Jonathan Concha (85876200)
1. `in_checkmate`
Purpose: Is used to determine if the current player is in checkmate.

Explanation:
```python
for square in self.squares:
    self.sq1 = square
    self.sq1_button = self.squares[square]
    ...
    if (self.turns % 2 == 0 and self.sq1_button["image"] not in self.white_pieces) or \
    (self.turns % 2 == 1 and self.sq1_button["image"] not in self.black_pieces):
        continue
```
- Checks to see which squares on the board have pieces belonging to the current player.
- Skips any square without a valid piece of the current player.

```python
for target_square in self.squares:
    self.sq2 = target_square
    self.sq2_button = self.squares[target_square]
    if self.allowed_piece_move() and not self.friendly_fire():
        ...
        if not self.in_check():
            return False
```
- Tests all possible moves for the current player's pieces.
- Simulates moves and checks if any move ends up with the king being in check.

```python
return True
```

2. `select_piece`
Purpose: Manages user interactions for selecting and moving pieces.

Explanation:
```python
if self.buttons_pressed == 0:
    if button["image"] in self.white_pieces and self.turns % 2 != 0:
        messagebox.showwarning(current_language["invalid_move"], current_language["invalid_move"])
        return
```
- Ensures the player can only select their own pieces during their turn.

```python
elif button["image"] not in self.white_pieces + self.black_pieces:
    messagebox.showwarning(current_language["invalid_selection"], current_language["invalid_selection"])
    return
```
- Prevents selecting an empty square or an invalid piece.

```python
if self.allowed_piece_move() and not self.friendly_fire():
    ...
    self.squares[self.sq2].config(image=self.sq1_button["image"])
    self.squares[self.sq1].config(image=self.white_images["blank.png"])
    ...
    if self.in_check():
        self.squares[prev_sq2].config(image=prev_sq2_button_piece)
        self.squares[prev_sq1].config(image=prev_sq1_button_piece)
        messagebox.showwarning(current_language["invalid_move"], current_language["invalid_move"])
        return
```
- Attempts to move the selected piece to the target square.
- If the move leaves the king in check, it is reverted, and the player is then warned.

Special Rules:
```python
if prev_sq1_button_piece == "pyimage3":  # White king
    self.wk_moved = True
```
- Tracks movements of key pieces like the king or rook for castling and check functions.

```python
if self.in_checkmate():
    winner = player1_name if self.turns % 2 != 0 else player2_name
    messagebox.showinfo(current_language["checkmate"], current_language["checkmate"].format(winner))
```
- Checks for checkmate after every move and announces the winner if detected.


3. `friendly_fire`
Purpose: To prevent a piece from capturing another piece of the same color.

Explanation:
```python
if self.piece_color == "white" and piece_2_color in self.white_pieces:
    return True
if self.piece_color == "black" and piece_2_color in self.black_pieces:
    return True
```
- If the target square contains a friendly piece, returns `True` to block the move from happening.


4. `allowed_piece_move`
Purpose: Checks if a move being attempted by a player is valid based on the selected piece's movement rules and game state.

Explanation:
```python
if self.sq1_button["image"] == wk or self.sq1_button["image"] == bk:  # King movement
    if (abs(int(self.sq1[1]) - int(self.sq2[1])) < 2) and (abs(self.ranks.find(self.sq1[0]) - self.ranks.find(self.sq2[0])) < 2):
        return True
```
- Validates that the king can only move one square in any direction at a time.

```python
if self.sq1_button["image"] == wp:  # White pawn movement
    if int(self.sq1[1]) + 1 == int(self.sq2[1]) and self.sq1[0] == self.sq2[0] and self.sq2_button["image"] == "pyimage2":
        return True
```
- Handles pawn movements, including single square moves and ensuring the destination is a valid movement based on its own rules.

```python
if (self.sq1_button["image"] == wq or self.sq1_button["image"] == bq) and self.clear_path("queen"):
    if int(self.sq1[1]) == int(self.sq2[1]) or self.sq1[0] == self.sq2[0]:
        return True
```
- Verifies that the queen’s movement is valid as a combination of rook and bishop moves so it can move as its supposed to.

```python
return False
```
- If none of the movement rules match, the function returns `False` and tells the player to select a valid move.



5. `castle`
Purpose: Handles the castling mechanics and validation.

Explanation:
```python
if self.wk_moved == False:
    if self.wr1_moved == False and self.sq2 == "c1":
        for x in range(1, 4):
            square_button = self.squares[self.ranks[x] + str(1)]
            if square_button["image"] != "pyimage2":
                return False
```
- Checks whether the king and rook involved in castling have moved and whether the squares between them are empty.

```python
self.squares["a1"].config(image="pyimage2")
self.squares["d1"].config(image="pyimage7")
```
- Updates the rook’s position on the board when the castling is mechanic performed.


6. `in_check`
Purpose: Determines if the king is under attack and has to move.

Explanation:
```python
if self.piece_color == "white":
    self.sq2 = self.find_king("pyimage3")
    for key in self.squares:
        self.sq1 = key
        self.sq1_button = self.squares[self.sq1]
        if self.sq1_button["image"] in self.black_pieces:
            if self.allowed_piece_move():
                return True
```
- Finds the king’s position and checks if any opponent piece can attack it while adhearing to the movement rules of that piece.

```python
return_previous_values()
return False
```
- Restores the previous state of the board and returns `False` if there are no checks found.

What to Improve: I think that I have to improve my ability to see which task I have to perform first, such as should I make sure this part of the code works before moving on
because this causes a lot of problems with time management, I think thats something that I definitely want to improve on.

What did I learn: I learned that python is a much easier language to understand than stuff like Java, I also learned that the frontend of a project and stuff like game fundamentals
are some of the most important part of coding because it sents the base of the game and how you will code the rest of your project.

What did I find difficult: The hardest part of the project was at the start when we were setting up the basics of chess like piece movement and attacking other pieces,
we had a bug that didnt let you capture any of the opponents pieces so we had to recode a lot of the project and functions inside the code.
