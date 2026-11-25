Windows programming assignment

PART 1: SOFTWARE DEVELOPMENT, METHODOLOGIES AND PRINCIPLES (CHAPTER 1)


1. Choosing the Right SDLC Model

Project A: flight-control software for a commercial aircraft. Recommended model: Waterfall, or Spiral combined with the V-model.

- Requirements stability: the requirements are fixed in advance by aviation safety regulations and must be fully specified, documented and traceable before coding starts, which is exactly what Waterfall's sequential phases provide.
- Risk tolerance: almost zero, because a failure can cost lives. Spiral adds an explicit risk analysis in every iteration so hazards are found and reduced early, and Waterfall adds formal sign-off at the end of each phase.
- Timing of testing: testing is planned from the beginning and every requirement is verified through extensive, formal, documented testing (unit, integration, system and certification testing). Releasing early and fixing later is not acceptable here.

Project B: food-delivery app for a startup in a fast-changing market. Recommended model: Agile/Scrum, or RAD.

- Requirements stability: requirements are unstable because customer needs, competitors and the market keep changing, so the process must welcome change. Agile does this through short sprints and a backlog that is reprioritized regularly.
- Risk tolerance: high, since a bug or a missing feature is annoying but not catastrophic and can be fixed in the next release. The bigger risk is being late to market or building the wrong product.
- Timing of testing: testing happens continuously, because each sprint produces a working increment that is tested and demonstrated, and user feedback arrives early. RAD's rapid prototyping serves the same purpose.

In short:

- High-risk systems with fixed requirements suit Spiral or Waterfall.
- Rapidly changing, customer-driven apps suit Agile/Scrum or RAD.
- The more stable the requirements and the lower the risk tolerance, the more upfront planning and formal late-stage testing is justified. The more volatile the requirements, the more we need short iterations and early, continuous testing.


2. The Hidden Trade-offs of Rapid Application Development

Skipping extensive upfront planning and relying on rapid prototyping causes scalability and reliability problems for these connected reasons:

- No architectural foundation: a prototype is built to show features quickly, not to handle thousands of users or large amounts of data. When the prototype slowly turns into the real product, its shortcuts (hard-coded values, tight coupling, a simple data model, no caching) become the architecture. This is architectural debt.
- Time-boxed delivery: the pressure to deliver visible features first means non-functional requirements such as performance, security, fault tolerance and maintainability are cut or postponed.
- Lack of deep risk analysis: problems like heavy load, data growth, integration limits and failure modes are discovered late, when fixing them means redesigning the system.
- High availability demands: mission-critical systems must run all the time with predictable behaviour. Quickly assembled components usually lack redundancy, thorough testing and proper error handling, so reliability suffers.
- Costly rework: scaling later requires rewriting core structure such as the database design and module boundaries, so the time saved at the start is lost later with interest.

RAD is therefore good for small, non-critical systems where speed matters, but for large or mission-critical systems the missing planning shows up as reduced scalability and reliability.


3. Balancing Software Development Principles

Yes, trying too hard to follow DRY or Separation of Concerns can violate KISS and YAGNI. These principles are guidelines with trade-offs and not goals in themselves, and over-applying one of them creates unnecessary complexity.

Hypothetical example: a console program that calculates the area of a rectangle.

- The simple solution is a single method that returns width times height.
- The over-engineered version would have an IShape interface, an abstract shape class, a ShapeFactory, an area calculator strategy, a service interface with an implementation, a repository layer, a dependency-injection container and even a configuration file for the formula, all split into separate presentation, business and data layers, for a program that has one formula.

Why this is a problem:

- It violates YAGNI because the program supports shapes, plug-ins and databases that nobody asked for and may never be needed.
- It violates KISS because a one-line calculation now spans a dozen files and classes, which makes it harder to read, debug and explain to a new developer.
- DRY and Separation of Concerns can be taken too far. Merging two pieces of code that only look similar into one generic abstraction, even though they change for different reasons, couples unrelated features, and splitting every tiny responsibility into its own layer adds indirection with no benefit.

A balanced approach is to start simple and add abstraction only when there is real duplication or a real present need, for example generalizing only after the same code has appeared about three times. Good engineering means choosing the simplest design that meets today's requirements while keeping it easy to change.


4. The Economics of Testing and Code Quality

Why the cost of correcting an error is lowest early and highest after deployment:

- An error found during requirements or design review only needs a document to be changed.
- If the same error reaches coding, then the code, its tests and the related documents all have to be changed.
- If it is found in system testing, the fix may touch many modules and everything has to be re-tested.
- After deployment the cost includes emergency patches, re-testing, redeployment, downtime, data corruption or recovery, customer support, lost revenue, damage to reputation and possibly legal problems.
- Defects also compound. An early mistake, such as a wrong requirement, spreads into the design and the code, and other code gets built on top of it, so everything that depends on it must be reworked. The cost therefore grows sharply with each phase.
- In the Waterfall model testing happens late, so defects pile up unnoticed and are expensive to untangle.

How modern practices keep the cost low:

- Test-Driven Development (TDD): tests are written before the code (write a failing test, make it pass, then refactor). This clarifies the requirement first, catches bugs within minutes of introducing them, and gives a test suite that protects against breaking things later.
- Continuous Integration: every commit is built and tested automatically, so integration problems are found within hours while the change is still small and fresh.
- Peer code reviews: other developers catch logic errors, design flaws and security holes before the code is merged, which is far cheaper than finding them in production. Reviews also spread knowledge in the team.


PART 2: .NET ARCHITECTURE, C# AND WINDOW PROGRAMMING (CHAPTER 2)


5. MSIL, JIT, and the Platform Independence Paradox

How compilation works:

- The C# or VB.NET compiler produces MSIL (Microsoft Intermediate Language) together with metadata, stored in an assembly (.exe or .dll).
- At run time the JIT compiler of the CLR converts the MSIL into native machine code for the specific CPU, such as x86, x64 or ARM, the first time each method is called.

Why the traditional .NET Framework is still platform-dependent on Windows even though MSIL is CPU-independent: CPU independence is not the same as operating system independence.

- The MSIL needs the CLR to run, and the .NET Framework's CLR was only implemented for Windows.
- The class libraries and application frameworks, such as Windows Forms, WPF and ASP.NET Web Forms, depend on Windows-specific APIs like Win32, GDI+, the registry, COM and the GAC.
- The JIT output and the runtime services for memory, threads and files are tied to Windows as well.
- So the intermediate code is portable, but the runtime and libraries it needs are not.

How Mono changed this:

- Mono is an open-source, third-party implementation of the CLR and the .NET class libraries, based on the ECMA standards for C# and the Common Language Infrastructure.
- It provides a runtime, JIT and libraries for Linux, macOS, Android, iOS and other systems, so the same MSIL assemblies can run outside Windows, with the Windows-specific APIs replaced or emulated.
- Mono later led to Xamarin, and Microsoft's own cross-platform .NET Core and modern .NET follow the same idea.


6. Managed vs. Unmanaged Code and Language Interoperability

Problems of unmanaged code: unmanaged code, such as C or C++ compiled straight to native code, gives the programmer full control but also full responsibility. Typical problems are memory leaks, dangling or invalid pointers, buffer overflows, uninitialized variables and type confusion, which can crash the process or open security holes.

How managed code under the CLR protects the program:

- Garbage collection: the garbage collector reclaims objects that are no longer reachable, which prevents most memory leaks, double frees and dangling pointers.
- Type safety: the CLR enforces that code can only access memory in valid, type-compatible ways, with no arbitrary pointer arithmetic unless the code is explicitly marked unsafe.
- Array bounds checking: accessing outside an array throws an IndexOutOfRangeException instead of silently overwriting memory, which stops buffer overflows.
- Metadata: it describes every type and member, which lets the CLR verify the code.
- Exception handling: the CLR provides one structured exception handling model for all languages.
- Isolation: code runs in a controlled environment, so a failing managed application does not corrupt other processes.

How MSIL and the CTS allow C# and VB.NET to work together:

- MSIL: whatever the language, the compiler produces the same MSIL and metadata. The CLR does not see C# or VB.NET, only MSIL, so a C# assembly and a VB.NET assembly can reference each other directly.
- Common Type System (CTS): it defines one shared set of data types and rules (value types, reference types, classes, interfaces, delegates and enums) that every .NET language follows, and language keywords are only aliases for CTS types. For example, string in C# and String in VB.NET both map to System.String, int and Integer both map to System.Int32, and every type derives from System.Object.
- Common Language Specification (CLS): a subset of rules that public members should follow so that they are usable from any language.
- Result: because both languages use the same types and rules, a team can write one part of an application in C# and another in VB.NET and pass data between them without conversion errors or type conflicts. A VB.NET class can even inherit from a C# class.


7. Solving "DLL Hell": Assemblies, Manifests and Namespaces

The problem: before .NET, applications shared DLLs that were placed in common folders such as System32 and registered in the registry. Installing one application could overwrite a shared DLL with an older or newer, incompatible version, which broke other applications that depended on it. Versions were not tracked reliably, and this problem was called DLL Hell.

How .NET solves it:

- Assemblies: the unit of deployment, versioning and security. An assembly is self-describing because it contains MSIL, metadata and a manifest.
- Manifest and metadata: the manifest lists the assembly's identity (name, version, culture and public key token), the files it consists of, and the exact versions of the other assemblies it depends on, so the CLR loads the correct dependencies without relying on the registry.
- Strong names: a strong name identifies an assembly uniquely by its name, version, culture and public key, and the assembly is digitally signed. Two different assemblies cannot share a strong name, and tampering is detected.
- Private assemblies: by default an application's assemblies are stored in the application's own folder, so no other application can overwrite them.
- Global Assembly Cache (GAC): assemblies that must be shared are strong-named and installed in the GAC, which can hold several versions of the same assembly at the same time.
- Side-by-side execution: because each application's manifest records the version it was built with, one application can use version 1.0 while another uses version 2.0 of the same assembly with no conflict.

Why namespace and assembly should not be used interchangeably:

- A namespace is a logical grouping of classes and types, used for organization and to avoid name conflicts, for example System.Collections.
- An assembly is a physical grouping, the actual .exe or .dll file that is the unit of deployment, versioning and security.
- One namespace can be spread over several assemblies, and one assembly can contain several namespaces.
- A using directive in code refers to a namespace, while a project reference refers to an assembly. Both are needed: the reference makes the assembly available and the using directive shortens the names.


8. Abstraction in Data Access: ADO.NET vs. Entity Framework and LINQ

Why raw DataSets and manual connections are error-prone:

- The developer must open and close connections, create commands, handle transactions and dispose resources by hand. Forgetting to close a connection leaks resources and can exhaust the connection pool.
- SQL is written as strings, so there is no compile-time checking. A typo in a table or column name only fails at run time, and building queries by string concatenation risks SQL injection.
- DataSets and DataTables are loosely typed. Values are read like row["Name"] and have to be cast and checked for DBNull, so type errors only appear at run time.
- The developer has to convert rows to objects and back manually, with a lot of repetitive mapping code, and keep the tables and classes in sync by hand.
- Different data sources such as SQL databases, XML and in-memory collections each need a different API.

What developers gain from Entity Framework (ORM) and LINQ:

- Automatic mapping of relational tables to .NET classes and rows to objects, so developers work with objects and properties instead of tables and rows.
- Strongly typed queries checked at compile time with IntelliSense support, translated into parameterized SQL, which also protects against SQL injection.
- LINQ standardizes querying with one syntax for SQL databases (LINQ to Entities), XML (LINQ to XML) and collections (LINQ to Objects).
- Built-in change tracking, relationships, migrations and transaction handling through SaveChanges, so there is much less boilerplate code.
- Higher productivity, easier maintenance and better database independence.

What may be traded off:

- Less control and some performance overhead. The generated SQL may be less efficient than hand-tuned SQL, and mistakes such as lazy loading causing many small queries (the N+1 problem) can hurt performance.
- A learning curve and some hidden behaviour, so developers who do not understand what happens underneath can write inefficient queries.
- The abstraction leaks, because complex queries, bulk operations or stored procedures may still need raw SQL or ADO.NET.
- Extra memory and start-up cost for the framework.

Overall, a higher level of abstraction improves productivity, safety and maintainability in exchange for some control and raw performance.


9. Under the Hood of Event-Driven Programming

Console app vs. Windows Forms app:

- A basic console app is sequential. Execution starts at Main(), the statements run in order, and the program exits when Main() returns, so the program controls the flow.
- A Windows Forms application is event-driven. Main() calls Application.Run(new Form1()), which starts the main loop, also called the message loop. After that the program mostly waits for something to happen, and the user controls the flow by clicking, typing and so on.

Key concepts:

- Event: a notification that something happened, such as the Click of a button. The publisher (the Button) raises it.
- Event listener (subscriber): code that registered its interest in that event.
- Delegate: a type-safe function pointer that defines the signature of the handler, for example EventHandler(object sender, EventArgs e). An event is basically a delegate with an invocation list that can hold many handlers.
- Event handler: the method that runs in response, such as button1_Click. It can be a named method or a lambda expression.

Sequence from the click to the code:

- At start-up, InitializeComponent() in the designer code subscribes the handler to the event, which adds the method to the event's invocation list:

    this.button1.Click += new EventHandler(this.button1_Click);
    // or with a lambda expression
    this.button1.Click += (sender, e) => MessageBox.Show("Clicked!");

- The user clicks the mouse on the button. The hardware and driver notify the operating system.
- Windows finds which window was clicked and puts a message, such as WM_LBUTTONDOWN and then WM_LBUTTONUP, into the message queue of the application's UI thread.
- The message loop inside Application.Run (GetMessage, TranslateMessage and DispatchMessage) takes the message out of the queue and dispatches it to the window procedure (WndProc) of the target control.
- The Button's WndProc recognizes a complete click and calls its OnClick method, which raises the Click event.
- The event invokes its delegate, which calls every subscribed handler in the invocation list and passes the sender (the button) and the EventArgs.
- The event handler code finally runs, for example showing a message box or updating a label.
- When the handler returns, control goes back to the message loop, which waits for the next message. This continues until the form is closed and Application.Exit posts WM_QUIT, which ends the loop.

So the chain is: click, OS message queue, main loop, the control's OnClick, the Click event, the delegate, and the event handler. Because the handler runs on the UI thread, a long-running handler freezes the form, since the message loop cannot process other messages until the handler finishes.
