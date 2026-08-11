---
categories:
- MS
- GitHub
date: '2026-08-10T19:30:00+00:00'
description: 'Java developers no longer have to rely on Java framework-specific approaches
  to drive AI from their enterprise apps.




  While it is true that Langchain4j empowe'
draft: false
original_url: https://github.blog/engineering/using-the-github-copilot-sdk-for-java/
source: GitHub Blog
tags:
- AI & ML
- Engineering
- GitHub Copilot SDK
- java
title: Using the GitHub Copilot SDK for Java
---

Java developers no longer have to rely on Java framework-specific approaches to drive AI from their enterprise apps.



While it is true that Langchain4j empowered developers by disintermediating specific AI vendors, you still had a dependency on Langchain4j. And with Spring AI, well, of course you had a dependency on design choices made by Spring, if not on Spring itself.



Now, GitHub Copilot SDK for Java is the first truly framework agnostic way to drive AI from Java. And with its BYOK support, GitHub Copilot SDK for Java is also AI vendor neutral.



&#128161; Even though it&rsquo;s called GitHub Copilot SDK, you can use it with any direct model provider, such as OpenAI, Azure, Anthropic, or OpenAI-compatible endpoints, by passing a provider/ProviderConfig with your own baseUrl + apiKey (or bearer token). No Copilot subscription required.



The GitHub Copilot SDK for Java is a client library that empowers your server-side Java code to create Copilot agent sessions, register tools, send prompts, and receive structured responses&mdash;all programmatically. It works in server environments, including Jakarta EE and Spring. If you&rsquo;ve been building enterprise Java for any length of time, this SDK will feel like home: CompletableFuture, annotations, lambdas, virtual threads, it&rsquo;s all here.



This post shows you how to use the SDK, walks through a complete Jakarta EE 11 sample application, and leaves you with concrete next steps to try it yourself. I chose Jakarta EE 11 for my demo because I was the lead release coordinator for that release. I believe in open standards as the best way to empower developers. For more on Jakarta EE 11 see this InfoQ article.



This sample app is an agent harness using Jakarta EE 11. But, of course, developers can build their own agent harness using the well-known Java frameworks and libraries of their choice.



Clone the sample app and try it yourself &gt;



Where to get it



The SDK is available as a Maven dependency:



&lt;dependency&gt;
    &lt;groupId&gt;com.github&lt;/groupId&gt;
    &lt;artifactId&gt;copilot-sdk-java&lt;/artifactId&gt;
    &lt;version&gt;1.0.7-preview.1&lt;/version&gt;
&lt;/dependency&gt;



Prerequisites:




JDK 17 or 25 (25 recommended &mdash; unlocks virtual threads and other modern features)



Maven 3.9+



A GitHub account with an active Copilot subscription



The Copilot CLI installed locally at version 1.0.71 or later.




Walk through the sample app



The best way to see the SDK in action is to run this sample application.



Get the code



git clone https://github.com/microsoft/Build26-BRK206-your-agent-anywhere-multiclient-multidevice-with-github-copilot-sdk.git
cd Build26-BRK206-your-agent-anywhere-multiclient-multidevice-with-github-copilot-sdk/src/java-agent-orchestrator
mvn clean package liberty:run
# Open http://localhost:9080/index.xhtml



The Java demo is built on:



ConcernTechnologyRuntimeOpen Liberty 26.0.0.5PlatformJakarta EE 11 (Faces 4.1, CDI 4.1, WebSocket 2.2, Data 1.0, Persistence 3.2)UIPrimeFaces 15.0.16AI orchestrationCopilot SDK for Java 1.0.7-preview.1DatabaseH2 in-memory (10 seed property listings)



What the app does



The application is a real-estate lead-management agent pipeline. A customer submits an enquiry (&ldquo;I&rsquo;m looking for a 3-bedroom house in London under &pound;800,000&rdquo;), and the system spins up an isolated Copilot Agent on a virtual thread to process it through a pipeline:







The architecture uses Jakarta WebSocket to push real-time status updates from the server to the browser, so you can watch agents progress through phases as the model calls tools:







Submit multiple inquiries simultaneously to see concurrent virtual-thread agents in action. Each one processes independently with its own Copilot session.











SDK features in action



Let&rsquo;s walk through the key SDK features as they appear in the sample code.



Defining tools with @CopilotTool



This is the headline API. If you&rsquo;ve ever written a @GET endpoint in JAX-RS or an @MessageDriven bean, this will feel instantly familiar:



@CopilotTool(value = "Sets the current phase of the agent. Use this to report progress.",
             name = "set_current_phase")
public String setCurrentPhase(
        @CopilotToolParam("The phase to transition to (VALIDATING, SEARCHING, "
                + "WRITING_REPORT, REJECTED_GARBAGE, REJECTED_NO_MATCHES, or DONE)")
        String phaseName) {
    phase = Phase.valueOf(phaseName.trim().toUpperCase(Locale.ROOT));
    notifyUi();
    return "Phase set to " + phase.getLabel();
}



The @CopilotTool annotation declares the method as a tool the model can call. The @CopilotToolParam annotation describes each parameter so the model knows what to pass. The SDK handles all the JSON Schema generation, argument parsing, and dispatch. You just write a normal Java method.



Two build prerequisites for @CopilotTool. The annotation-based tool API is currently an experimental feature of the SDK, so you need to configure two things in your Maven build:




Enable experimental APIs: pass -Acopilot.experimental.allowed=true to the compiler. Without this flag, the annotation processor will refuse to generate the tool metadata. For more details on the experimental APIs see Copilot SDK documentation.



Register the annotation processor: add the SDK as an annotationProcessorPath so the compiler can find the @CopilotTool processor and generate the $$CopilotToolMeta classes at compile time.




Both are configured in the maven-compiler-plugin:



&lt;plugin&gt;
    &lt;groupId&gt;org.apache.maven.plugins&lt;/groupId&gt;
    &lt;artifactId&gt;maven-compiler-plugin&lt;/artifactId&gt;
    &lt;version&gt;3.15.0&lt;/version&gt;
    &lt;configuration&gt;
        &lt;compilerArgs&gt;
            &lt;arg&gt;-Acopilot.experimental.allowed=true&lt;/arg&gt;
        &lt;/compilerArgs&gt;
        &lt;annotationProcessorPaths&gt;
            &lt;path&gt;
                &lt;groupId&gt;com.github&lt;/groupId&gt;
                &lt;artifactId&gt;copilot-sdk-java&lt;/artifactId&gt;
                &lt;version&gt;1.0.7-preview.1&lt;/version&gt;
            &lt;/path&gt;
        &lt;/annotationProcessorPaths&gt;
    &lt;/configuration&gt;
&lt;/plugin&gt;



To register all annotated tools from an object:



List&lt;ToolDefinition&gt; annotatedTools = ToolDefinition.fromObject(this);



Inline lambda tools with ToolDefinition.from(...)



When you want a tool defined at the call site without a dedicated method, use the lambda style:



ToolDefinition reportIntentTool = ToolDefinition
        .from("report_intent",
              "Reports the current intent of the agent",
              Param.of(String.class, "intent", "Intent in max 4 words"),
              (String intent) -&gt; {
                  currentIntent = intent;
                  addEvent(Instant.now(), "intent", "Intent updated", intent);
                  notifyUi();
                  return "ok";
              })
        .overridesBuiltInTool(true);



Notice .overridesBuiltInTool(true). This tells the SDK that our report_intent tool deliberately replaces a built-in tool of the same name. This is useful when you need custom behaviour for a tool the model already knows about.



Cross-class tool scanning



Tools don&rsquo;t have to live in the same class as your agent logic. Here&rsquo;s searchProperties defined in a separate CDI bean:



@ApplicationScoped
public class PropertyDatabase {

    @CopilotTool(value = "Searches the real estate listings database. "
                       + "Returns up to 10 matching properties.",
                 name = "search_properties")
    public List&lt;Property&gt; searchProperties(
            @CopilotToolParam("Property type substring (e.g. 'flat', 'house')") String type,
            @CopilotToolParam("City substring (e.g. 'London', 'Bristol')") String city,
            @CopilotToolParam("Minimum number of bedrooms (0 for no minimum)") int minBedrooms,
            @CopilotToolParam("Maximum price in GBP (0 for no maximum)") double maxPriceGbp) {
        // ... filter and return matching properties ...
    }
}



You would normally register these with ToolDefinition.fromObject(propertyDatabase). In the sample app, we use a lambda wrapper instead, because CDI client proxies can obscure the annotation metadata.



Customizing the system message



The SDK gives you fine-grained control over the system message. Use SystemMessageMode.CUSTOMIZE to replace specific sections while preserving the rest:



SystemMessageConfig systemMessage = new SystemMessageConfig()
        .setMode(SystemMessageMode.CUSTOMIZE)
        .setSections(Map.of(SystemMessageSections.IDENTITY,
            new SectionOverride()
                .setAction(SectionOverrideAction.REPLACE)
                .setContent("""
                    You are part of a real estate recommendation system.
                    You will receive enquiries from customers, and you must
                    carry out the following workflow...
                    """)));



The text block ("""...""") makes multi-line prompts readable without string concatenation. The IDENTITY section override replaces only the model&rsquo;s self-description while leaving safety guardrails intact. If you prefer a simpler approach, SystemMessageMode.APPEND adds your content after the default system message without replacing anything.



The agentic loop: sendAndWait(...)



One line kicks off the full agentic loop:



session = client.createSession(sessionConfig).get();
// ...
AssistantMessageEvent result = session.sendAndWait(escapedEnquiry).get();



Behind .get(), the model reasons, calls your tools (potentially multiple times), and returns its final response. On a virtual thread, .get() is cheap. No platform thread is consumed while waiting. The SDK dispatches tool calls to your registered handlers automatically and feeds results back to the model until it&rsquo;s done.



Real-time event handling with session.on(...)



Subscribe to session events to build responsive UIs:



sessionSubscription = session.on(event -&gt; {
    captureSessionEvent(event);
    uiUpdateSocket.pushDetailUpdate(id);
});



Every tool call, every result, every assistant message fires an event. The sample app captures these events and pushes them to the browser via Jakarta WebSocket, so the pipeline dashboard updates in real time. You can use pattern matching to handle specific event types:



if (event instanceof AssistantMessageEvent msg) {
    finalReport = msg.getData().content();
} else if (event instanceof ToolExecutionStartEvent start) {
    // Tool is being invoked...
}



Headless client and permission handling



The client is configured for server-side operation:



copilotClient = new CopilotClient(
        new CopilotClientOptions()
                .setMode(CopilotClientMode.EMPTY)
                .setCopilotHome(copilotHome)
                .setExecutor(contextualVirtualThreadExecutor));



CopilotClientMode.EMPTY means no IDE integration &mdash; the client talks directly to the Copilot CLI. The custom Executor (discussed below) ensures tool callbacks run with container context.



For permission handling, the sample uses:



sessionConfig.setOnPermissionRequest(PermissionHandler.APPROVE_ALL);



APPROVE_ALL is appropriate for demos and development. In production, implement a real permission policy that validates which tools the model is allowed to invoke.



Jakarta EE integration patterns



The SDK is not a framework island. It composes naturally with Jakarta EE &mdash; and of course also with proprietary frameworks such as Spring.



The Executor parameter is the key integration point. Jakarta Concurrency (&sect;5.2 in the 3.1 spec) requires that application-created threads be obtained from a ManagedThreadFactory so the container can:




Track the thread for lifecycle shutdown (@PreDestroy / server stop)



Apply concurrency constraints and policies



Propagate context automatically (without needing manual contextualRunnable)




Open Liberty 26.x supports virtual-thread ManagedThreadFactory via the virtual attribute in server.xml.



&lt;managedThreadFactory jndiName="concurrent/virtualThreadFactory" virtual="true" /&gt;



Then, in AppState.java we inject the factory:



@Resource(lookup = "concurrent/virtualThreadFactory")
private ManagedThreadFactory virtualThreadFactory;



And use it to create the Executor we pass to the Copilot SDK.



// The ManagedThreadFactory (virtual=true) creates container-managed virtual
// threads that automatically propagate CDI, JNDI, and transaction context.
Executor managedVirtualExecutor = runnable -&gt;
    virtualThreadFactory.newThread(runnable).start()

String copilotHome = Path.of(System.getProperty("user.home"), ".copilot").toString();
CopilotClientOptions copilotClientOptions = new CopilotClientOptions()
        .setMode(CopilotClientMode.EMPTY)
        .setCopilotHome(copilotHome)
        .setExecutor(managedVirtualExecutor);
copilotClient = new CopilotClient(copilotClientOptions);



This creates virtual threads that carry the container&rsquo;s context. When the SDK dispatches a tool call to searchProperties(), that method can @Inject a JPA repository and query the database, because the container context is present on the callback thread.



Other integration patterns in the sample:




CDI @ApplicationScoped for the singleton CopilotClient (one client per application lifecycle).



Jakarta Faces f:websocket push for real-time browser updates via PushContext.



Jakarta Data @Repository for type-safe database queries without raw JPA boilerplate.




Fine-grained tool access control with ToolSet. The SessionConfig lets you specify exactly which tools each session can access:



sessionConfig.setAvailableTools(new ToolSet()
        .addCustom("*")           // all registered custom tools
        .addBuiltIn("web_fetch")); // only the web_fetch built-in



This is an important production concern. Rather than exposing every built-in tool (file system access, shell execution, etc.), you explicitly opt in to only what the agent needs. In the sample app, we allow all custom tools plus web_fetch so the agent can look up real-time property information during the Search phase.



Summary



Here&rsquo;s what we covered:




Java-native API: CompletableFuture, annotations, lambdas, and virtual threads make the SDK feel like idiomatic Java, not a ported-from-another-language afterthought.



Three tool-definition styles: annotations for enterprise patterns, lambdas for inline convenience, JSON Schema for full control.



System message customization: section-level overrides give you precise control over agent behaviour.



The agentic loop in one line: sendAndWait(...) handles the full tool-calling loop automatically.



Real-time event streaming: session.on(...) enables responsive UIs and observability.



Headless server-side operation: no IDE required; runs anywhere the Copilot CLI is available.



Natural composition with Jakarta EE: CDI, JPA, WebSocket, and virtual threads all work together through the Executor integration point.




What to try next




Explore the BYOK support. The GitHub Copilot SDK can be used directly against model providers, for example OpenAI, Azure, Anthropic, or OpenAI-compatible endpoints, by passing a provider/ProviderConfig with your own baseUrl + apiKey (or bearer token). No Copilot subscription required.



Clone the sample app and run it locally. Submit multiple enquiries simultaneously to see virtual threads in action.



Swap the model. Try session.setModel(...) to experiment with different Copilot models.



Add your own tool. Define a new @CopilotTool method (a mortgage calculator, a school-district lookup) and watch the agent discover and use it.



Deploy to Azure. Open Liberty runs great on Azure App Service, AKS, or Azure Container Apps. See the Jakarta EE on Azure guidance at https://aka.ms/java/ee.




The Copilot SDK for Java puts the full power of GitHub Copilot behind your Java code with no IDE required and no framework lock-in.




Clone the sample app and try it yourself &gt;


The post Using the GitHub Copilot SDK for Java appeared first on The GitHub Blog.

---
*원문: [https://github.blog/engineering/using-the-github-copilot-sdk-for-java/](https://github.blog/engineering/using-the-github-copilot-sdk-for-java/)*
