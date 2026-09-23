<div align="center">
# Blackjack 
**A Java desktop card game built to explore object-oriented programming.**
Java · Swing / AWT · Java Sound · Single-player
Quick start · Gameplay · Architecture · Known limitations
[Tên hiển thị](URL)
</div>
![Blackjack game table with betting chip, balance, score counters, and gameplay controls](OOP-Project-main/images/img_2.png)
> Screenshot supplied with the project. The current source has known asset and gameplay issues described below.
Overview
Blackjack is a single-player desktop game developed as an Object-Oriented Programming project at International University, Vietnam National University Ho Chi Minh City. Play against a computer-controlled dealer, choose a bet, draw cards, and track your balance and round wins during the session.
The project combines a 52-card deck, hand-value calculation, mouse-driven Swing components, and audio playback. Its custom rules include a minimum standing total of 15 and a five-card condition, so its behavior differs from standard casino Blackjack.
Features
Graphical interface: starting-balance screen, main menu, game table, Help, and About Us dialogs.
Card and deck model: 52 cards across four suits, shuffled for each new round.
Player actions: Hit, Stand, and Exit through on-screen buttons.
Bet selection: 1, 5, 10, 25, 100, or All In, with a check against the available balance.
Hand evaluation: number-card values, face cards worth 10, and Aces adjusted from 11 to 1 when needed.
Round outcomes: opening Blackjack checks, dealer comparisons, ties, and five-card checks.
Session feedback: balance display, player/dealer win counters, background audio, and sound effects.
All balances are in-game values held in memory. The opening screen collects a balance; it does not authenticate a user or create an account.
Quick start
Requirements
A Java Development Kit (JDK) with both `java` and `javac` available on your terminal path. The source uses Java 8 language features; Java 8 or later is required at the language level.
A desktop environment for Swing windows and an available audio device for sound playback.
The included `images/` and `sounds/` folders.
The project imports only Java standard-library APIs. No Maven, Gradle, database, or third-party Java dependencies are required by the supplied source.
1. Open the project folder
Download and extract this repository, or clone it using its GitHub Code menu. Open a terminal in the repository root—the folder containing this README and `OOP-Project-main`—then run:
```sh
cd OOP-Project-main
java -version
javac -version
```
Keep this directory as the working directory when launching the application. Asset paths such as `images/icon.png` are resolved relative to it.
2. Correct the missing card-image reference
The supplied `src/Card.java` loads `images/backsideOfACard.jpg`, but that file is absent. The included card-back asset is `images/backside\_card.png`.
Before running, change this line in `src/Card.java`:
```java
BufferedImage backOfACard = ImageIO.read(new File("images/backsideOfACard.jpg"));
```
To:
```java
BufferedImage backOfACard = ImageIO.read(new File("images/backside\_card.png"));
```
This read occurs even when drawing a face-up card. Without the correction, the image-loading exception can prevent both player and dealer cards from rendering. This README documents the correction; the supplied Java source is otherwise unchanged.
3. Compile and run
From `OOP-Project-main`, run these commands in PowerShell, Command Prompt, or a Unix shell:
```sh
mkdir out
javac -encoding UTF-8 -d out src/\*.java
java -cp out Tester
```
If `out` already exists, skip the first command. Re-run the compile command after editing Java files.
VS Code: open `OOP-Project-main` as the workspace folder and run the commands above in the integrated terminal. If using an IDE Run configuration instead, set its working directory to `OOP-Project-main` and its main class to `Tester`.
> Validation status: the documentation was checked against the supplied source and asset filenames. Compilation and interactive gameplay have not been verified in the review environment because `javac` was unavailable.
Gameplay
Enter a positive whole-number starting balance, such as `1000`, and select START.
Choose PLAY from the main menu. HELP displays the in-game instructions; ABOUT US displays the project team.
Click the chip on the left side of the table and select a bet within your balance.
Use HIT to draw another card or STAND to resolve the round when your total is at least 15.
Continue with another round, or select EXIT to close the game and display your remaining balance.
Rules implemented in the source
Rule	Current behavior
Deck	A fresh shuffled 52-card deck is created for each round.
Initial deal	Player and dealer each receive two cards; dealer cards are initially hidden.
Card values	Cards 2–10 use their face value; J, Q, and K count as 10.
Aces	Count as 11, then reduce by 10 per Ace as needed to avoid exceeding 21 when possible.
Standing	Allowed at a player total of 15 or higher. Some in-game text incorrectly refers to 14.
Dealer draws	The dealer draws while its total is 14 or lower.
Opening Blackjack	The code checks the dealer's opening hand, then the player's, for a total of 21. Simultaneous Blackjack needs correction.
Five-card condition	A five-card hand of 21 or less is checked for a special win; the player's exactly-21 case has conflicting checks.
Comparison	Equal totals or both hands over 21 produce a tie in the Stand handler; otherwise bust status and hand totals determine the result.
Balance settlement	The main session balance changes by one bet on a win or loss and stays unchanged on a tie. No special 3:2 Blackjack payout is implemented.
See Known limitations before relying on edge-case outcomes.
Architecture
The application separates card data, deck operations, game decisions, custom drawing, menu interaction, and sound playback across seven classes.
Class	Responsibility
`Tester`	Application entry point; creates windows, collects the starting balance, tracks session state, and runs refresh/round-check threads.
`Card`	Stores suit, rank, and value; draws a card using the sprite sheet or card-back image.
`Deck`	Builds the 52-card collection and provides shuffle, access, and removal operations.
`Game`	Manages hands, deals cards, handles Hit/Stand, evaluates totals, and records round outcomes.
`GameComponent`	Draws the table, cards, balance, and scores; handles chip clicks and bet selection.
`OptionsComponent`	Draws the main menu and handles Play, Help, Quit, and About Us actions.
`SE`	Loads and controls audio clips through `javax.sound.sampled`.
Object-oriented concepts
Encapsulation: `Card` keeps its card properties private and exposes getters; `Deck` owns its private card collection.
Composition: a `Deck` contains `Card` objects, while `Game` manages a deck and two hands.
Inheritance and interfaces: the two UI components extend `JComponent` and implement `MouseListener`.
Method overriding: custom components provide painting and mouse-event behavior; thread subclasses override `run()`.
Singleton-style access: `Tester.getInstance()` provides a shared `Tester` instance, although much of the application state remains static.
Game logic and UI behavior still share state through `Tester` and `GameComponent`; separating them further is an opportunity for refactoring.
Repository contents
Path	Contents
`OOP-Project-main/src/`	Seven Java source files; entry point: `Tester.java`.
`OOP-Project-main/images/`	Card graphics, backgrounds, buttons, and project screenshots.
`OOP-Project-main/sounds/`	Background music and sound assets.
`OOP-Project-main/BLACKJACK-PROJECT.pdf`	Included project report.
Open the project report.
<details>
<summary>View the starting-balance screen</summary>
![Starting-balance screen with an input field and Start button](OOP-Project-main/images/img.png)
</details>
Known limitations
This is an educational prototype. Source review identified the following issues:
Missing card asset reference: `Card.java` requests a filename absent from the repository. Apply the correction in Quick start.
Input validation: non-numeric or out-of-range balance input can throw `NumberFormatException`; zero and negative balances are not rejected at entry.
Bet lifecycle: dismissing the bet dialog can start a zero-value round. Repeated chip clicks are not guarded against starting another deal during an active round.
Five-card conflict: a player with exactly five cards totaling 21 meets both the win (`<= 21`) and loss (`>= 21`) conditions.
Opening Blackjack conflict: dealer and player opening checks run sequentially without stopping after the first outcome, so simultaneous Blackjack is not resolved as a clean tie.
Bust handling: Hit does not consistently end the round immediately when the player exceeds 21; some outcomes wait for Stand or the five-card checks.
Threading and performance: refresh and round-check threads run continuous loops without a delay. Swing changes also occur outside the Event Dispatch Thread, and some handlers block it with `Thread.sleep`.
Asset and audio handling: images are loaded repeatedly during painting, exceptions are often suppressed, and failed audio initialization can leave `SE.clip` null.
Persistence and testing: balances and scores reset when the application closes. No automated tests or build-tool configuration are included.
Suggested improvements
[ ] Resolve the asset path and round-ending edge cases.
[ ] Validate starting balances and canceled bets; allow only one active round.
[ ] Extract hand scoring and round resolution into independently testable classes.
[ ] Replace busy loops with event-driven updates and a Swing timer where needed.
[ ] Load images once and report resource-loading failures clearly.
[ ] Add tests for multiple Aces, busts, ties, opening Blackjack, and five-card hands.
Team
Contributor	Student ID
Nguyễn Nguyên Hiệu	ITDSIU21087
Lê Xuân Tâm	ITDSIU21118
Nguyễn Ngọc Sang	ITDSIU21114
Course context: Object-Oriented Programming, International University, VNU-HCM.
Contributing
Issues and pull requests are welcome. For a bug report, include your operating system, Java version, steps to reproduce, and any terminal error output. Keep changes focused and describe how you checked the resulting behavior.
License and assets
The previous README mentioned the MIT License, but no `LICENSE` file is included in the supplied repository. License terms need to be confirmed and added by the maintainers. Image and audio permissions should also be documented separately where applicable. reach out to: 
- Nguyenhieu24lk@gmail.com
- Lexuantam761311@gmail.com
- Nguyenngocsang051003@gmail.com
