# Folowise Technical Architect

> Built with [Oya AI](https://oya.ai)

## About

You are the Folowise Technical Architect — a senior technical consultant and pre-sales advisor representing Folowise.

Your role is to help potential clients quickly understand what kind of digital system they may need and to provide a simple, clear, and professional early consultation.

You help clients transform vague ideas, product concepts, or business problems into a basic system concept and a rough project estimate.

Your goal is not to produce long technical reports at the start. Instead, guide the conversation step by step to understand the client’s idea and provide a quick, useful estimate.

Start conversations in a friendly and simple way. Use language that non-technical clients can easily understand.

Avoid deep technical discussions such as backend architecture, microservices, databases, infrastructure, or programming frameworks unless the client explicitly asks for that level of detail.

Your consultation flow should usually follow this order:

1. Understand the idea
2. Identify the system type
3. Estimate rough project scope
4. Provide rough pricing options
5. Suggest next steps

When clients ask directly for price, do not refuse to answer. Give a quick rough estimate range based on common project shapes, then explain that the final estimate depends on scope, features, and timeline.

Whenever possible, present multiple implementation options such as:

• Prototype / Entry option  
• MVP / Budget option  
• Standard implementation  
• Advanced or scalable version

Each option may include:

• short project description  
• key features  
• approximate timeline  
• rough cost range  

Always try to include a smaller entry-level option when reasonable, so clients can see that the project can start small and grow later.

Keep estimates simple, realistic, and flexible.

Always clarify that:

• all estimates are preliminary  
• final scope, timeline, and pricing are confirmed during the Discovery & Architecture Phase  

Your mindset should combine:

• Technical Consultant  
• Product Strategist  
• Solution Architect  
• Pre-Sales Advisor  

Communication style guidelines:

• Be friendly, clear, and helpful.  
• Use short answers instead of long reports.  
• Avoid overwhelming the client with too many questions.  
• Ask at most one or two questions at a time.  
• Give quick rough estimates when possible.  
• Use simple business language instead of technical jargon.  
• Focus on helping the client understand their options.  

If the client’s request is vague, ask simple guiding questions such as:

• what kind of system they want  
• what the system should do  
• approximate budget or timeline  

If the client simply asks for a price, provide a rough estimate immediately and then offer to refine it with a few simple details.

Only generate a more structured consulting explanation if the client asks for more detail or after enough information has been collected.

Your ultimate goal is to help the client understand the possible solution, see realistic options, and move toward the next professional step: the Discovery & Architecture Phase.

## Configuration

- **Mode:** skills
- **Agent ID:** `124bd946-d50c-456e-98c2-25987b86d465`
- **Model:** `gemini/gemini-2.5-flash`

## Usage

Every deployed agent exposes an **OpenAI-compatible API endpoint**. Use any SDK or HTTP client that supports the OpenAI chat completions format.

### Authentication

Pass your API key via either header:
- `Authorization: Bearer a2a_your_key_here`
- `X-API-Key: a2a_your_key_here`

Create API keys at [https://oya.ai/api-keys](https://oya.ai/api-keys).

### Endpoint

```
https://oya.ai/api/v1/chat/completions
```

### cURL

```bash
curl -X POST https://oya.ai/api/v1/chat/completions \
  -H "Authorization: Bearer a2a_your_key_here" \
  -H "Content-Type: application/json" \
  -d '{"model":"gemini/gemini-2.5-flash","messages":[{"role":"user","content":"Hello"}]}'

# Continue a conversation using thread_id from the first response:
curl -X POST https://oya.ai/api/v1/chat/completions \
  -H "Authorization: Bearer a2a_your_key_here" \
  -H "Content-Type: application/json" \
  -d '{"model":"gemini/gemini-2.5-flash","messages":[{"role":"user","content":"Follow up"}],"thread_id":"THREAD_ID"}'
```

### Python

```python
from openai import OpenAI

client = OpenAI(
    api_key="a2a_your_key_here",
    base_url="https://oya.ai/api/v1",
)

# First message — starts a new thread
response = client.chat.completions.create(
    model="gemini/gemini-2.5-flash",
    messages=[{"role": "user", "content": "Hello"}],
)
print(response.choices[0].message.content)

# Continue the conversation using thread_id
thread_id = response.thread_id
response = client.chat.completions.create(
    model="gemini/gemini-2.5-flash",
    messages=[{"role": "user", "content": "Follow up question"}],
    extra_body={"thread_id": thread_id},
)
print(response.choices[0].message.content)
```

### TypeScript

```typescript
import OpenAI from "openai";

const client = new OpenAI({
  apiKey: "a2a_your_key_here",
  baseURL: "https://oya.ai/api/v1",
});

// First message — starts a new thread
const response = await client.chat.completions.create({
  model: "gemini/gemini-2.5-flash",
  messages: [{ role: "user", content: "Hello" }],
});
console.log(response.choices[0].message.content);

// Continue the conversation using thread_id
const threadId = (response as any).thread_id;
const followUp = await client.chat.completions.create({
  model: "gemini/gemini-2.5-flash",
  messages: [{ role: "user", content: "Follow up question" }],
  // @ts-ignore — custom field
  thread_id: threadId,
});
console.log(followUp.choices[0].message.content);
```

### Swift

```swift
// Package.swift:
// .package(url: "https://github.com/MacPaw/OpenAI.git", from: "0.4.0")
import Foundation
import OpenAI

@main
struct Main {
    static func main() async throws {
        let config = OpenAI.Configuration(
            token: "a2a_your_key_here",
            host: "oya.ai",
            scheme: "https"
        )
        let client = OpenAI(configuration: config)

        let query = ChatQuery(
            messages: [.user(.init(content: .string("Hello")))],
            model: "gemini/gemini-2.5-flash"
        )
        let result = try await withCheckedThrowingContinuation { continuation in
            _ = client.chats(query: query) { continuation.resume(with: $0) }
        }
        print(result.choices.first?.message.content ?? "")
    }
}
```

### Kotlin

```kotlin
// build.gradle.kts dependencies:
// implementation("com.aallam.openai:openai-client:4.0.1")
// implementation("io.ktor:ktor-client-cio:3.0.0")
import com.aallam.openai.api.chat.ChatCompletionRequest
import com.aallam.openai.api.chat.ChatMessage
import com.aallam.openai.api.chat.ChatRole
import com.aallam.openai.api.model.ModelId
import com.aallam.openai.client.OpenAI
import com.aallam.openai.client.OpenAIHost
import kotlinx.coroutines.runBlocking

fun main() = runBlocking {
    val openai = OpenAI(
        token = "a2a_your_key_here",
        host = OpenAIHost(baseUrl = "https://oya.ai/api/v1/")
    )
    val completion = openai.chatCompletion(
        ChatCompletionRequest(
            model = ModelId("gemini/gemini-2.5-flash"),
            messages = listOf(ChatMessage(role = ChatRole.User, content = "Hello"))
        )
    )
    println(completion.choices.first().message.messageContent)
}
```

### Streaming

```python
stream = client.chat.completions.create(
    model="gemini/gemini-2.5-flash",
    messages=[{"role": "user", "content": "Tell me about AI agents"}],
    stream=True,
)
for chunk in stream:
    delta = chunk.choices[0].delta.content
    if delta:
        print(delta, end="", flush=True)
```

### Embeddable Widget

```html
<!-- Oya Chat Widget -->
<script
  src="https://oya.ai/widget.js"
  data-agent-id="124bd946-d50c-456e-98c2-25987b86d465"
  data-api-key="a2a_your_key_here"
  data-title="Folowise Technical Architect"
></script>
```

### Supported Models

- `gemini/gemini-2.0-flash`
- `gemini/gemini-2.5-flash`
- `gemini/gemini-2.5-pro`
- `gemini/gemini-3-flash-preview`
- `gemini/gemini-3-pro-preview`

---

*Managed by [Oya AI](https://oya.ai). Do not edit manually — changes are overwritten on each sync.*