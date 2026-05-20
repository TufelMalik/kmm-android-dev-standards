# SOLID Principles Rules

> All code must follow SOLID principles for maintainability and testability.

## S - Single Responsibility

```kotlin
// ✅ DO - One class, one job
class UserRepository { /* only data operations */ }
class UserValidator { /* only validation */ }

// ❌ DON'T - Multiple responsibilities
class UserManager { 
    fun saveUser() { }
    fun validateUser() { }
    fun sendEmail() { }
}
```

## O - Open/Closed

```kotlin
// ✅ DO - Open for extension, closed for modification
interface PaymentProcessor { fun process(amount: Double) }
class CreditCardProcessor : PaymentProcessor { }
class UpiProcessor : PaymentProcessor { }

// ❌ DON'T - Modify existing code for new types
fun process(type: String) {
    when (type) {
        "credit" -> { }
        "upi" -> { }
        // Adding new type requires modifying this
    }
}
```

## L - Liskov Substitution

```kotlin
// ✅ DO - Subtypes are substitutable
open class Bird { open fun move() { } }
class Sparrow : Bird() { override fun move() { fly() } }
class Penguin : Bird() { override fun move() { walk() } }

// ❌ DON'T - Subtype breaks parent contract
open class Bird { open fun fly() { } }
class Penguin : Bird() { override fun fly() { throw Exception() } }
```

## I - Interface Segregation

```kotlin
// ✅ DO - Small, focused interfaces
interface Readable { fun read() }
interface Writable { fun write() }
class File : Readable, Writable { }

// ❌ DON'T - Fat interfaces
interface FileOperations {
    fun read()
    fun write()
    fun delete()
    fun compress()
}
```

## D - Dependency Inversion (MANDATORY)

> All dependencies MUST be interfaces, never concrete classes. NO direct `Impl` creation.

```kotlin
// ✅ DO - Depend on interface, inject via DI
interface UserRepository { fun getUser(): User }

class UserViewModel(private val repo: UserRepository) { }

// DI Module (Koin)
val appModule = module {
    single<UserRepository> { UserRepositoryImpl() }
    viewModel { UserViewModel(get()) }
}

// ❌ DON'T - Concrete dependency, direct creation
class UserViewModel {
    private val repo = UserRepositoryImpl()  // INVALID
}
```

## ✅ DO

- One class = one responsibility
- Use interfaces for abstractions
- Inject dependencies via constructor
- Create small, focused interfaces

## ❌ DON'T

- Don't create god classes
- Don't hardcode dependencies
- Don't force unused interface methods
