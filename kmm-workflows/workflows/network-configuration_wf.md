---
description: Configuring network
---

# Network Configuration

> **Context**: Setting up HTTP client. One client, timeouts required.

## Setup

```kotlin
val networkModule = module {
    single {
        HttpClient {
            install(ContentNegotiation) { json() }
            install(HttpTimeout) {
                requestTimeoutMillis = 30_000
            }
            defaultRequest { url(ApiConfig.BASE_URL) }
        }
    }
}

object ApiConfig {
    const val BASE_URL = "https://api.example.com/"
}
```

## Rules

> **Must follow:** [network-client-rules](../rules/network-client-rules.md)
