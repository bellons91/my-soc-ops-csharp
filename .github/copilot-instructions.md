# Copilot Instructions for SocOps

## Mandatory Development Checklist
- [ ] `dotnet format --verify-no-changes` (lint/format gate)
- [ ] `dotnet build SocOps/SocOps.csproj`
- [ ] `dotnet test` (run when tests exist; none currently in this repo)

## Architecture
- Blazor WebAssembly (.NET 10) with one core flow: Start -> Playing board -> Bingo modal.
- `SocOps/Pages/Home.razor` orchestrates state-driven UI and subscribes/unsubscribes `BingoGameService.OnStateChanged`.
- Bootstrap and routing are in `SocOps/Program.cs` and `SocOps/App.razor`.

## Service Boundaries
- `BingoGameService` owns mutable game state, localStorage persistence (`bingo-game-state`, versioned), and change notifications.
- `BingoLogicService` owns gameplay rules only (`GenerateBoard`, `ToggleSquare`, `CheckBingo`, winning IDs).
- Keep `SocOps/Models/*` shapes stable because they are serialized to localStorage.

## UI and Data Patterns
- Components are presentational + callback-driven: `StartScreen` (`OnStart`), `GameScreen` (board props + actions), `BingoBoard` -> `BingoSquare`, `BingoModal` (`OnDismiss`).
- Question source is `SocOps/Data/Questions.cs`; center free space is fixed at index `12` and pre-marked.
- Saves are intentionally fire-and-forget (`_ = SaveGameStateAsync()`) to avoid blocking interactions.

## Workflow and Styling
- Run locally: `dotnet run --project SocOps/SocOps.csproj` (`http://localhost:5166`).
- Styling uses utility classes in `SocOps/wwwroot/css/app.css`; compose existing utilities before adding small single-purpose ones.
