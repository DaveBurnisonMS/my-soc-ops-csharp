# Project Guidelines

## Mandatory Development Checklist
- [ ] Lint: `dotnet format --verify-no-changes SocOps/SocOps.csproj`
- [ ] Build: `dotnet build SocOps/SocOps.csproj`
- [ ] Test: `dotnet test vscode-agent-lab-soc-ops-csharp.sln`
- [ ] If no tests are present, explicitly note that in your summary and perform manual gameplay smoke checks.

## Code Style
- Use existing C# and Razor naming: PascalCase, descriptive names, nullable-safe code.
- Keep UI components parameter-driven; put orchestration/state transitions in services.
- Use utility classes from `SocOps/wwwroot/css/app.css`; avoid ad-hoc inline styles.

## Architecture
- App: Blazor WebAssembly (`net10.0`) under `SocOps/`.
- `SocOps/Pages/Home.razor` orchestrates page state.
- `SocOps/Services/BingoGameService.cs` owns game state + localStorage persistence.
- `SocOps/Services/BingoLogicService.cs` contains deterministic, side-effect-free bingo logic.
- Models live in `SocOps/Models/`; questions in `SocOps/Data/Questions.cs`.

## Build and Test
- Run app: `dotnet run --project SocOps/SocOps.csproj` (or `cd SocOps && dotnet run`).
- No dedicated test project currently exists; rely on manual bingo flow checks when tests are absent.

## Conventions
- `BingoGameService` is the single source of truth; call `NotifyStateChanged()` after mutations.
- Keep subscribe/unsubscribe lifecycle paired to prevent duplicate renders.
- Preserve gameplay assumptions unless intentionally changed: 5x5 board, free center (pre-marked), row/column/diagonal win detection.
- Maintain localStorage compatibility (`bingo-game-state`, versioned payload).

## Frontend and Styling
- Follow `.github/instructions/frontend-design.instructions.md` for design tasks.
- Preserve current visual language unless a redesign is explicitly requested.
- Add minimal utilities to `SocOps/wwwroot/css/app.css` when needed.

## Pitfalls
- Fire-and-forget saves (`_ = SaveGameStateAsync()`) can fail silently.
- UI rerenders depend on service state notifications.
- Free space is intentionally non-clickable.

## Read First
- `README.md`
- `SocOps/Pages/Home.razor`
- `SocOps/Services/BingoGameService.cs`
- `SocOps/Services/BingoLogicService.cs`
- `SocOps/wwwroot/css/app.css`