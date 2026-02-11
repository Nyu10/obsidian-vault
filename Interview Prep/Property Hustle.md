


Domain -> heart of application
	business entities and logic
	zero dependencies on external frameworks

port interfaces
	abstract definitions of how data enter or leaves the core

adapters
	glue that translates technical requests into a language core understands 

driver inbound
	rest API 
driven outbound
	external tools application needs
	database
	message queue



"So I used Hexagonal Architecture, also called Ports and Adapters. The key idea is the domain sits in the center [draw hexagon] and has ZERO dependencies on the outside world."

"Inbound adapters [draw left side in green] push data INTO the domain—React sends a WebSocket message, the controller converts it to a command, and it goes through the UseCase PORT—that's just an interface."

"Outbound adapters [draw right side in orange] are how the domain talks to infrastructure—it publishes events through a PORT, which an adapter implements to broadcast state via WebSocket."

"The beautiful thing is: if I want to swap WebSocket for REST, or add a CLI, I just write a new adapter. The domain never changes."


"A port is just an interface that defines a contract. In Hexagonal Architecture, we use the term 'port' to emphasize that it's a boundary—the domain only knows about the interface, never the implementation. This lets us swap implementations (like switching from in-memory to Postgres) without touching the core logic."
	testing super easy
		replace dependencies very easily


public interface GameUseCase {
GameState createGame(BotGameRequest request);
GameState getGameState(String roomId);
GameState processMove(String roomId, Move move);
// New Command-based API
GameState executeCommand(String gameId, GameCommand command);

}





"I used **Hexagonal Architecture**, also called Ports and Adapters.

My **domain layer** contains pure game logic - rent calculation, win conditions, bot AI. It has **zero framework dependencies**. I can unit test it in isolation.

My **application layer** orchestrates use cases - it fetches game state, calls domain logic, saves results.

My **infrastructure layer** handles HTTP, WebSockets, and database access. The controllers talk to the application layer through **interfaces called ports** - like `GameUseCase` - so I can swap implementations without touching business logic.

The benefit? **My game rules don't know they're running in a Spring app**. They'd work the same in a CLI, a different web framework, or a unit test."

![[Screenshot 2026-01-16 at 2.34.21 PM.png]]

What I'm working on
	UI Changes
	converting from vercel to use docker and probably switch to OCI for free always on 
		vercel is more for serverless functions, but my websockets don't work well on it






Questions for him 
healthcare is notoriously slow and bloated
how do you think yuzu will continue to capture the broader market

what are the most difficult technical challenges right now that you'd think i'd be working on if i join


