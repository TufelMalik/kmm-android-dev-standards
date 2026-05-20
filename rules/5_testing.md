# Rule 5: Testing Best Practices

> **Official References:**  
> - [Android Testing Guide](https://developer.android.com/training/testing)
> - [Testing Compose](https://developer.android.com/jetpack/compose/testing)
> - [Android Testing Codelab](https://developer.android.com/codelabs/advanced-android-kotlin-training-testing-basics)
> - [JUnit 5](https://junit.org/junit5/docs/current/user-guide/)

---

## Rule: Write Comprehensive Tests at Multiple Levels

**Why:** Tests catch bugs early, enable confident refactoring, document expected behavior, and reduce manual testing time. A good test suite is your safety net for code changes.

**Official Best Practice:** Follow the Test Pyramid - more unit tests, fewer integration tests, minimal UI tests.

---

## Test Pyramid

```
        ┌─────────────────┐
        │    UI Tests     │  10-20%
        │     (E2E)       │
        ├─────────────────┤
        │  Integration    │  20-30%
        │     Tests       │
        ├─────────────────┤
        │   Unit Tests    │  50-70%
        │                 │
        └─────────────────┘
```

---

## Test File Locations

```
app/
├── src/
│   ├── main/                    # Production code
│   ├── test/                    # Unit tests (JVM)
│   │   └── java/com/yourpackage/
│   │       ├── viewmodel/
│   │       │   └── LoginViewModelTest.kt
│   │       └── repository/
│   │           └── TaskRepositoryTest.kt
│   └── androidTest/             # Instrumented tests (device/emulator)
│       └── java/com/yourpackage/
│           ├── ui/
│           │   └── LoginScreenTest.kt
│           └── database/
│               └── TaskDaoTest.kt
```

**For KMM:**
```
composeApp/src/
├── commonMain/           # Shared code
├── commonTest/           # Shared tests
├── androidTest/          # Android instrumented tests
└── jvmTest/              # JVM tests
```

---

## 1. Unit Tests

Test individual functions, classes, or ViewModels in isolation.

### ViewModel Test Example

```kotlin
// LoginViewModelTest.kt
package com.yourpackage.ui.screens.login

import kotlinx.coroutines.test.*
import org.junit.Before
import org.junit.Test
import org.junit.Assert.*
import io.mockk.*

class LoginViewModelTest {
    
    private lateinit var authRepository: AuthRepository
    private lateinit var viewModel: LoginViewModel
    
    @Before
    fun setup() {
        authRepository = mockk()
        viewModel = LoginViewModel(authRepository)
    }
    
    @Test
    fun `login with valid credentials returns success`() = runTest {
        // Given
        val email = "test@example.com"
        val password = "password123"
        coEvery { authRepository.login(email, password) } returns Result.success(User("123"))
        
        // When
        viewModel.login(email, password)
        
        // Then
        assertTrue(viewModel.uiState.value.isSuccess)
        coVerify { authRepository.login(email, password) }
    }
    
    @Test
    fun `login with empty email shows error`() {
        // Given
        val email = ""
        val password = "password123"
        
        // When
        viewModel.login(email, password)
        
        // Then
        assertNotNull(viewModel.uiState.value.emailError)
        assertEquals("Email cannot be empty", viewModel.uiState.value.emailError)
    }
    
    @Test
    fun `login failure shows error message`() = runTest {
        // Given
        val email = "test@example.com"
        val password = "wrong"
        coEvery { authRepository.login(email, password) } returns 
            Result.failure(Exception("Invalid credentials"))
        
        // When
        viewModel.login(email, password)
        
        // Then
        assertTrue(viewModel.uiState.value.isError)
    }
}
```

### Repository Test Example

```kotlin
// TaskRepositoryTest.kt
package com.yourpackage.data.repository

import kotlinx.coroutines.test.runTest
import org.junit.Before
import org.junit.Test
import org.junit.Assert.*
import io.mockk.*

class TaskRepositoryTest {
    
    private lateinit var taskDao: TaskDao
    private lateinit var repository: TaskRepository
    
    @Before
    fun setup() {
        taskDao = mockk()
        repository = TaskRepositoryImpl(taskDao)
    }
    
    @Test
    fun `getTasks returns list from dao`() = runTest {
        // Given
        val tasks = listOf(Task(1, "Task 1"), Task(2, "Task 2"))
        coEvery { taskDao.getAllTasks() } returns tasks
        
        // When
        val result = repository.getTasks()
        
        // Then
        assertEquals(tasks, result)
        coVerify { taskDao.getAllTasks() }
    }
    
    @Test
    fun `saveTask calls dao insert`() = runTest {
        // Given
        val task = Task(1, "New Task")
        coEvery { taskDao.insert(task) } just Runs
        
        // When
        repository.saveTask(task)
        
        // Then
        coVerify { taskDao.insert(task) }
    }
}
```

---

## 2. Integration Tests

Test interactions between components (Repository + Database).

```kotlin
// TaskDaoTest.kt
package com.yourpackage.data.local

import androidx.room.Room
import androidx.test.core.app.ApplicationProvider
import androidx.test.ext.junit.runners.AndroidJUnit4
import kotlinx.coroutines.test.runTest
import org.junit.After
import org.junit.Before
import org.junit.Test
import org.junit.runner.RunWith
import org.junit.Assert.*

@RunWith(AndroidJUnit4::class)
class TaskDaoTest {
    
    private lateinit var database: AppDatabase
    private lateinit var taskDao: TaskDao
    
    @Before
    fun setup() {
        // In-memory database for testing
        database = Room.inMemoryDatabaseBuilder(
            ApplicationProvider.getApplicationContext(),
            AppDatabase::class.java
        ).allowMainThreadQueries().build()
        
        taskDao = database.taskDao()
    }
    
    @After
    fun teardown() {
        database.close()
    }
    
    @Test
    fun insertTask_retrievesTask() = runTest {
        // Given
        val task = Task(id = 1, title = "Test Task", completed = false)
        
        // When
        taskDao.insert(task)
        val retrieved = taskDao.getTaskById(1)
        
        // Then
        assertEquals(task, retrieved)
    }
    
    @Test
    fun deleteTask_removesFromDatabase() = runTest {
        // Given
        val task = Task(id = 1, title = "Test Task", completed = false)
        taskDao.insert(task)
        
        // When
        taskDao.delete(task)
        val retrieved = taskDao.getTaskById(1)
        
        // Then
        assertNull(retrieved)
    }
    
    @Test
    fun updateTaskCompletion_updatesInDatabase() = runTest {
        // Given
        val task = Task(id = 1, title = "Test Task", completed = false)
        taskDao.insert(task)
        
        // When
        taskDao.updateCompletion(1, true)
        val retrieved = taskDao.getTaskById(1)
        
        // Then
        assertTrue(retrieved?.completed == true)
    }
}
```

---

## 3. UI Tests (Compose)

Test UI behavior and user interactions.

```kotlin
// LoginScreenTest.kt
package com.yourpackage.ui.screens.login

import androidx.compose.ui.test.*
import androidx.compose.ui.test.junit4.createComposeRule
import org.junit.Rule
import org.junit.Test

class LoginScreenTest {
    
    @get:Rule
    val composeTestRule = createComposeRule()
    
    @Test
    fun loginScreen_displaysAllElements() {
        composeTestRule.setContent {
            LoginScreen(
                onLoginClick = {},
                onForgotPasswordClick = {}
            )
        }
        
        // Verify UI elements exist
        composeTestRule.onNodeWithText("Login").assertIsDisplayed()
        composeTestRule.onNodeWithTag("email_field").assertIsDisplayed()
        composeTestRule.onNodeWithTag("password_field").assertIsDisplayed()
        composeTestRule.onNodeWithText("Sign In").assertIsDisplayed()
        composeTestRule.onNodeWithText("Forgot Password?").assertIsDisplayed()
    }
    
    @Test
    fun emailField_acceptsInput() {
        composeTestRule.setContent {
            LoginScreen(onLoginClick = {}, onForgotPasswordClick = {})
        }
        
        // Enter text
        composeTestRule.onNodeWithTag("email_field")
            .performTextInput("test@example.com")
        
        // Verify text is displayed
        composeTestRule.onNodeWithTag("email_field")
            .assertTextContains("test@example.com")
    }
    
    @Test
    fun loginButton_triggersCallback() {
        var loginClicked = false
        
        composeTestRule.setContent {
            LoginScreen(
                onLoginClick = { loginClicked = true },
                onForgotPasswordClick = {}
            )
        }
        
        // Click login button
        composeTestRule.onNodeWithText("Sign In").performClick()
        
        // Verify callback was triggered
        assertTrue(loginClicked)
    }
    
    @Test
    fun emptyEmail_showsError() {
        composeTestRule.setContent {
            LoginScreen(onLoginClick = {}, onForgotPasswordClick = {})
        }
        
        // Enter only password
        composeTestRule.onNodeWithTag("password_field")
            .performTextInput("password123")
        
        // Click login
        composeTestRule.onNodeWithText("Sign In").performClick()
        
        // Verify error is shown
        composeTestRule.onNodeWithText("Email cannot be empty").assertIsDisplayed()
    }
}
```

### Compose UI Component Test

```kotlin
// CustomButtonTest.kt
package com.yourpackage.ui.components

import androidx.compose.ui.test.*
import androidx.compose.ui.test.junit4.createComposeRule
import org.junit.Rule
import org.junit.Test

class CustomButtonTest {
    
    @get:Rule
    val composeTestRule = createComposeRule()
    
    @Test
    fun button_displaysText() {
        composeTestRule.setContent {
            CustomButton(
                text = "Click Me",
                onClick = {}
            )
        }
        
        composeTestRule.onNodeWithText("Click Me").assertIsDisplayed()
    }
    
    @Test
    fun button_clickTriggersCallback() {
        var clicked = false
        
        composeTestRule.setContent {
            CustomButton(
                text = "Click Me",
                onClick = { clicked = true }
            )
        }
        
        composeTestRule.onNodeWithText("Click Me").performClick()
        assertTrue(clicked)
    }
    
    @Test
    fun button_disabledState_cannotClick() {
        var clicked = false
        
        composeTestRule.setContent {
            CustomButton(
                text = "Click Me",
                onClick = { clicked = true },
                enabled = false
            )
        }
        
        composeTestRule.onNodeWithText("Click Me").performClick()
        assertFalse(clicked)
    }
}
```

---

## Test Naming Convention

**Pattern:** `methodName_stateUnderTest_expectedBehavior`

```kotlin
@Test
fun login_withValidCredentials_returnsSuccess() { }

@Test
fun addTask_withEmptyTitle_showsValidationError() { }

@Test
fun deleteButton_whenClicked_removesItem() { }

@Test
fun loadTasks_onNetworkError_showsErrorMessage() { }
```

---

## Dependencies (build.gradle.kts)

```kotlin
dependencies {
    // Unit Testing
    testImplementation("junit:junit:4.13.2")
    testImplementation("org.jetbrains.kotlinx:kotlinx-coroutines-test:1.7.3")
    testImplementation("io.mockk:mockk:1.13.8")
    testImplementation("app.cash.turbine:turbine:1.0.0") // Flow testing
    
    // Android Instrumented Tests
    androidTestImplementation("androidx.test.ext:junit:1.1.5")
    androidTestImplementation("androidx.test.espresso:espresso-core:3.5.1")
    androidTestImplementation("androidx.compose.ui:ui-test-junit4:1.5.4")
    debugImplementation("androidx.compose.ui:ui-test-manifest:1.5.4")
    
    // Room Testing
    androidTestImplementation("androidx.room:room-testing:2.6.0")
}
```

---

## Running Tests

```bash
# Run all unit tests
./gradlew test

# Run specific test class
./gradlew test --tests LoginViewModelTest

# Run all Android instrumented tests
./gradlew connectedAndroidTest

# Run tests with coverage
./gradlew testDebugUnitTestCoverage
```

---

## Best Practices Summary

✅ Test business logic in ViewModels (unit tests)  
✅ Test database operations (integration tests)  
✅ Test critical user flows (UI tests)  
✅ Use test doubles (mocks, fakes) for dependencies  
✅ Follow AAA pattern: Arrange, Act, Assert  
✅ Write descriptive test names  
✅ Aim for 70-80% code coverage  
✅ Keep tests fast and isolated  
✅ Run tests in CI/CD pipeline  
✅ Add `testTag` to Compose elements for testing  

---

*Following these practices ensures reliable, well-tested code that can be refactored with confidence.*
