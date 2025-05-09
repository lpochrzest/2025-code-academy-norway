# AI-Powered .NET Development with Cursor - Workshop Instructions

**Goal:** Learn how to leverage the Cursor IDE's AI features to accelerate common .NET development tasks using a layered console application.

**Prerequisites:**
*   Cursor IDE installed.
*   Basic understanding of C# and .NET concepts (classes, methods, interfaces).
*   The sample C# Library Console Application project open in Cursor.

---

## 1. Introduction to Cursor IDE (5 mins)

Familiarize yourself with the key AI features in the Cursor UI:

*   **Chat Panel (Right):** (`Ctrl+L`/`Cmd+L`) This is your main AI interaction hub. Notice the dropdown at the top - this selects the Chat **Mode** (e.g., Agent, Ask). We'll primarily use **Agent mode** for more complex tasks. Ensure 'Agent' is selected for relevant exercises.
*   **Chat Context (`@`):** Use the `@` symbol in the chat input to provide specific context for focusing the AI, such as specific files (`@file`), folders (`@folder`), symbols (`@symbol`), or web search (`@web`). To reference the whole project, you can frame your prompt to imply project-wide context or ensure no specific context is active.
*   **Inline Actions (`Cmd+K` / `Ctrl+K`):** Select code or place your cursor, press `Cmd+K` (Mac) or `Ctrl+K` (Windows/Linux), and type a prompt for focused generation, editing, or explanation of code. This is a primary way to interact with the AI for specific code blocks.
*   **Hover Actions (for Diagnostics):** Hover over code underlined by Cursor's linter/diagnostics (e.g., errors or warnings) to see details. You may see options like "Fix in chat" to have the AI help resolve the issue. General "Explain" or "Edit" options are more reliably accessed via `Cmd+K` or the Chat Panel.
*   **Diagnostics:** Look for underlined code for potential issues. Hovering often provides details and may offer AI-assisted fixes (see Hover Actions).
*   **Right-Click Menu:** When you right-click on code or a file, the context menu offers various options. For AI interactions, you'll often find "Add Symbol to Current Chat" or "Add Symbol to New Chat", which allows you to quickly bring the selected code into the Chat Panel for further actions like explaining, generating documentation, or tests. Direct "Chat", "Generate Docs/Tests" options might not always be present as top-level items but are achieved through the Chat Panel once context is added.

**Action:**
1.  Open the Chat panel (`Ctrl+L`/`Cmd+L`). Ensure **Agent mode** is selected. Ask a project-wide question like: `What is the purpose of this project?` (Cursor will use its understanding of the codebase, potentially enhanced by codebase indexing, as context for such general queries.)
2.  Select any line of code and press `Cmd+K` (Mac) or `Ctrl+K` (Windows/Linux) to see the inline prompt. Try typing `Explain this line`.

---

## 2. Analyze and Document Existing Code (10 mins)

Use Cursor to understand and document code.

**Task 1: Explain Code**

1.  Open a C# file with some logic (e.g., `src/Library.Console/ConsoleApp.cs` or `src/Library.ApplicationCore/Services/LoanService.cs`).
2.  Select an entire method (like `PatronSearch()` in `ConsoleApp.cs` or `ReturnLoan()` in `LoanService.cs`).
3.  Right-click the selection and choose an option like 'Add Symbol to Current Chat' or 'Add Symbol to New Chat'. This will bring the selected code into the Chat Panel. Alternatively, select the code and press `Ctrl+L`/`Cmd+L` to open Chat with the selection as context.
4.  In the chat prompt (ensure **Agent mode** or **Ask mode** is suitable), type: `Explain this method step-by-step. What does it do?` Review the explanation.
5.  Select another piece of code (e.g., the `JsonData` class definition in `src/Library.Infrastructure/Data/JsonData.cs`).
6.  Press `Ctrl+K` / `Cmd+K` and type: `Explain this code.` Observe the inline explanation.

**Task 2: Generate Documentation**

1.  In the Chat panel (ensure **Agent mode**), clear any specific file/code context.
2.  Ask a question or give a command that implies project-wide scope, for example: `Generate a README.md file for this project. Describe its main purpose (Library Console App), structure (list key folders like src/Library.ApplicationCore, src/Library.Infrastructure, src/Library.Console, tests/UnitTests), and how to run it (e.g., using 'dotnet run' from the src/Library.Console directory).`
3.  Review the proposed README content and click the button to create/update the file.
4.  Go back to one of the service class files (e.g., `src/Library.ApplicationCore/Services/LoanService.cs`).
5.  Place your cursor *above* the `ReturnLoan()` method definition.
6.  Press `Ctrl+K` / `Cmd+K` and type: `Add XML documentation comments for this method.`
7.  Review and accept the generated `/// <summary>...` comments. (Note: XML comments are less critical for console apps than APIs but still good practice).

---

## 3. Develop New Code Features (35 mins)

Use Cursor's **Agent mode** and **Cmd+K** to add new functionality to the console app.

**Task: Add Book Search Functionality**

1.  **Plan:**
    *   Go to the Chat panel. For this initial planning step where we want to outline the tasks without immediate implementation, select **Ask mode** from the mode dropdown. (If you were to use Agent mode for planning, you would need to be very explicit in your prompt to request *only* a plan and to wait for approval before writing any code.)
    *   Ensure you are in the main project directory context (or clear any specific `@` context in chat). For existing files like `@file:src/Library.Console/ConsoleApp.cs`, you can add them as context if needed. For the new repository files we'll be creating (like `IBookRepository.cs` in `src/Library.ApplicationCore/Interfaces/` and `JsonBookRepository.cs` in `src/Library.Infrastructure/Data/`), keep these names and paths in mind as we'll instruct the AI to create them shortly.
    *   In **Ask mode**, ask: `Outline the steps needed to add functionality to search for books by title in this console application. This should involve: 1. Defining a search method in an IBookRepository interface. 2. Implementing this method in a JsonBookRepository class (similar to JsonPatronRepository). 3. Adding a new state and corresponding logic in ConsoleApp.cs to prompt the user for a book title search term, call the repository method, and display the results (list of matching book titles and authors). Assume JsonBookRepository needs access to JsonData.`
    *   Review the suggested plan. Cursor might suggest creating the interface/class if they don't exist or modifying existing ones.
2.  **Generate Repository Method (if needed):**
    *   If `IBookRepository` or `JsonBookRepository` don't exist or lack the search method:
    *   Use Chat (**Agent mode**), ensuring project-wide context (clear any specific `@` file/symbol contexts): `Create an IBookRepository interface in src/Library.ApplicationCore/Interfaces with an async method SearchBooks that takes a string searchInput and returns Task<List<Book>>. Then create a JsonBookRepository class in src/Library.Infrastructure/Data that implements IBookRepository. It should take JsonData via constructor injection, ensure data is loaded, search the Books list in JsonData based on the title containing the searchInput (case-insensitive), populate the Author for each found book using JsonData's helper methods, sort by title, and return the list. Register IBookRepository/JsonBookRepository in Program.cs.`
    *   Review and accept the generated/modified files.
3.  **Generate Console App Logic:**
    *   Go to `src/Library.Console/ConsoleApp.cs`.
    *   Place your cursor where new state logic might fit or use Chat (**Agent mode**) with `@file:src/Library.Console/ConsoleApp.cs @file:src/Library.Console/ConsoleState.cs`.
    *   Prompt: `Add a new state 'BookSearch' to the ConsoleState enum. Modify the main loop in ConsoleApp.cs to handle this state. Create a new async method BookSearch() similar to PatronSearch(). This method should: inject IBookRepository, prompt the user for a book title search term, call _bookRepository.SearchBooks(), display the list of matching books (e.g., "1) Book Title by Author Name"), handle cases where no books are found, and transition to a new state 'BookSearchResults' (create this state too). Create the BookSearchResults() method which prompts the user to start a new book search ('b'), patron search ('s') or quit ('q'), and transitions accordingly.`
    *   Review the generated code carefully (new enum values, switch case, new methods, repository usage, display logic). Accept the changes. Ensure `IBookRepository` is added to the `ConsoleApp` constructor and assigned.
4.  **Refine (If Necessary):**
    *   If the search needs refinement (e.g., ensuring case-insensitivity wasn't handled correctly):
    *   Select the search logic within `JsonBookRepository.SearchBooks`.
    *   Press `Ctrl+K` / `Cmd+K` or use Chat (`@code selection`, ensure **Agent mode**).
    *   Prompt: `Make the book title search case-insensitive.`
    *   Review and accept the changes.

---

## 4. Develop Unit Tests (25 mins)

Generate unit tests using **Agent mode** for existing business logic.

**Task: Generate Unit Tests for `PatronService.RenewMembership`**

*   *(Setup Note: The project already includes an xUnit test project and uses NSubstitute for mocking.)*

1.  **Generate Tests:**
    *   Go to the `src/Library.ApplicationCore/Services/PatronService.cs` file.
    *   Select the entire `RenewMembership` method.
    *   Open the Chat panel (`Ctrl+L` or `Cmd+L`).
    *   Ensure the context is `@code selection` and **Agent mode** is selected.
    *   Type the prompt: `Generate xUnit tests for the selected C# method (PatronService.RenewMembership). Place these tests in a new file named RenewMembershipTests.cs under tests/UnitTests/ApplicationCore/Services/ or add them to an existing PatronServiceTests.cs file if one exists in that path. Use NSubstitute to mock IPatronRepository. Include test cases for: successful renewal, patron not found, renewal attempted too early (more than 1 month before expiry), and patron has overdue loans preventing renewal. Use existing project factories (PatronFactory, LoanFactory) if possible for test data.`
    *   **Instruction:** Review the file changes proposed by Cursor's Agent mode. This will likely involve Agent creating a new test file (e.g., `tests/UnitTests/ApplicationCore/Services/RenewMembershipTests.cs`) or modifying an existing one. Examine the generated test code: the test class, `[Fact]` methods, arrange/act/assert structure, and mock setups.
    *   **Action:** Accept the proposed file changes if they look correct. After accepting, you might still need to add or adjust `using` statements if the AI missed any, or make other minor refinements to the code.
2.  **Add More Tests:**
    *   In the chat (ensure **Agent mode**, context might be the generated tests or select the test class code), ask: `Add another test case to check if the MembershipEnd date is correctly updated by exactly one year upon successful renewal.`
    *   Review the new test method (it should check the `MembershipEnd` property value after a successful call).
    *   Integrate the new test method into your test class.
    *   *(Optional)* Try running the tests (`dotnet test` from the solution root or via Test Explorer).

---

## 5. Refactor and Improve Code (20 mins)

Use Cursor to proactively identify areas for improvement and refactoring in your code. This can involve using the Chat panel for in-depth reviews or `Cmd+K`/`Ctrl+K` for targeted changes.

**Task 1: Proactively Review Code for Improvements**

1.  Go to one of your C# classes that you think might benefit from review (e.g., `src/Library.Infrastructure/Data/JsonData.cs` or `src/Library.Console/ConsoleApp.cs`).
2.  Select a significant block of code or an entire method/class that you want the AI to analyze for potential refactoring, performance enhancements, or adherence to best practices.
3.  Open the Chat panel (`Ctrl+L`/`Cmd+L`). If the selected code isn't automatically added as context, you can right-click the selection and choose 'Add Symbol to Current Chat' or use `@code selection` in the chat input.
4.  Ensure **Agent mode** or **Ask mode** is selected. Ask a question focused on improvement, for example: `Review this code for potential improvements. Are there opportunities for refactoring to improve readability or maintainability? Are there any performance considerations or deviations from C# best practices I should be aware of? Could any parts be more concise or use modern C# features more effectively?`
5.  Review the suggestions provided by the AI. It might suggest specific refactorings, alternative approaches, or point out areas that could be optimized.
6.  (Additionally) As you work, also be mindful of any code underlined by Cursor's diagnostics. Hovering over these can provide quick details and may sometimes offer AI-assisted fixes for more direct issues.

**Task 2: Apply Refactoring (Using `Cmd+K` / `Ctrl+K`)**

1.  Select a piece of code (e.g., a LINQ query or a loop in `JsonPatronRepository.SearchPatrons` or the new `JsonBookRepository.SearchBooks`).
2.  Press `Ctrl+K` / `Cmd+K`. This is often best for targeted refactoring.
3.  Type: `Can this loop be converted to a more efficient LINQ expression?` or `Improve the readability of this code block.`

---

## Wrap-up

You have now practiced using Cursor AI for:
*   Code Explanation in a console application.
*   Documentation Generation (README, XML Comments by framing project-wide prompts, Ctr+K for methods/classes).
*   Feature Development (Planning, Repository/Console Logic Generation, Refinement).
*   Unit Test Generation for service layer logic.
*   Code Review and Refactoring.

**Key Tips:**
*   Be specific with your prompts.
*   Provide context by framing your prompts clearly (for project-wide scope) or by using `@` symbols for specific elements like files (`@file:`).
*   Remember the different Chat Modes (Agent, Ask, Manual) - we primarily used **Agent** today.
*   **Always review and test AI-generated code.**
*   Iterate on prompts if the first result isn't perfect. 