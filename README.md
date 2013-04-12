TTT
===
### Tic Tac Toe

import random

### The Start Grid as a variable 'grid'
def Grid():
  grid = [1, 2, 3,
			4, 5, 6,
			7, 8, 9]
	return grid		


### Print Grid on Display
def Pgrid(grid):
	print("------------")
	print("| " + str(grid[1-1]) + " | " + str(grid[2-1]) + " | " + str(grid[3-1]) + " |")
	print("------------")
	print("| " + str(grid[4-1]) + " | " + str(grid[5-1]) + " | " + str(grid[6-1]) + " |")
	print("------------")
	print("| " + str(grid[7-1]) + " | " + str(grid[8-1]) + " | " + str(grid[9-1]) + " |")
	print("------------")


### Check a tuple(spot1, spot2, spot3) to have the same symbol 'char' (from char-acter)
def CheckWin(grid, char, spot1, spot2, spot3):
	if char == grid[spot1-1] and char == grid[spot2-1] and char == grid[spot3-1]:
		return True


### Check if player or PC is the winner. 'Char' is a symbol ('X' or 'O')
def CheckAll(grid, char):
	if CheckWin(grid, char, 1, 2, 3):     ## Upper Raw
		return True
	elif CheckWin(grid, char, 4, 5, 6):   ## Middle Raw
		return True
	elif CheckWin(grid, char, 7, 8, 9):   ## Lower Raw
		return True
	elif CheckWin(grid, char, 1, 4, 7):   ## Left Column
		return True
	elif CheckWin(grid, char, 2, 5, 8):   ## Middle Column
		return True
	elif CheckWin(grid, char, 3, 6, 9):   ## Right Column
		return True
	elif CheckWin(grid, char, 1, 5, 9):   ## Diagonal
		return True
	elif CheckWin(grid, char, 3, 5, 7):   ## Diagonal
		return True
	else:
		return False


""" Player chose his/her symbol, i.e X or O
	First symbol is the player, Second one is the pc """
def ChoseChar():
	char = ""	
	while char != "X" and char != "O":
		char = raw_input("Chose your symbol (X or O) ")
		char = char.upper()
	if char == "X":
		return ("X", "O")
	else:
		return ("O", "X")


### Chose who starts the game first (player or pc)
def FirstMove():
	first = ""
	while not (first.startswith("Y") or first.startswith("N")):
		first = raw_input("Do you want to have a first move? (Y or N) ")
		first = first.upper()
		if first.startswith("Y"):
			return (1, 0)
		else:
			return (0, 1)


### If player wants to play again the function returns True, otherwise False
def PlayAgain():
	input = raw_input("Do you want to play again? (Yes or No) ")
	return input.upper().startswith("Y")


### Refresh the Grid for next move
def RefGrid(grid, possiblemove, move, char):
	grid[move - 1] = char
	possiblemove.remove(move)
	return (grid, possiblemove)
	

### PlayerMove
def PlayerMove(grid, possiblemove):
	Pgrid(grid)
	move = 10
	while int(move) not in possiblemove:
		move = raw_input("Select a spot? " + str(possiblemove) + " ")
	move = int(move)
	grid, possiblemove = RefGrid(grid, possiblemove, move, player)
	return (grid, possiblemove, move)


### PcMove
def PcMove(grid, possiblemove):
	print("Computer's move")
### Check if the computer can win in the next move
	move = ""
	leftover = possiblemove[:]
	for i in leftover:
		copyGrid = grid[:]
		copyGrid[i - 1] = pc
		if CheckAll(copyGrid, pc):
			return i
### Check if the player can win in the next move and take that spot
	for i in leftover:
		copyGrid = grid[:]
		copyGrid[i - 1]	= player
		if CheckAll(copyGrid, player):
			return i
## Take the center - 5
	if 5 in leftover:
		return 5			
### Take a corner - 1, 3, 7, 9
	for i in [1, 3, 7, 9]:
		leftpossible = []
		if i in leftover:
			leftpossible.append(i)
		if leftpossible != []:
			return random.choice(leftpossible)
### Take a side - 2,4,6,8
	for i in [2, 4, 6, 8]:
		leftpossible = []
		if i in leftover:
			leftpossible.append(i)
		if leftpossible != []:
			return random.choice(leftpossible)	


############# Start Game ####################################
print("Tic Tac Toe")
player, pc = ChoseChar()
turn_player, turn_pc = 1, 0 #### turn - player starts 1st,  PC starts 2nd

while True:
	grid = Grid()
	possiblemove = [1, 2, 3, 4, 5, 6, 7, 8, 9]
	win = ""
	while len(possiblemove) > 0:
		if turn_player == len(possiblemove) % 2:
			grid, possiblemove, move = PlayerMove(grid, possiblemove)
			if CheckAll(grid, player):
				Pgrid(grid)
				print("Congratulations! You are the winner!")
				win = player
				urn_player, turn_pc = 1, 0 #### turn - player starts 1st,  PC starts 2nd
				break	
		else:
			move = PcMove(grid, possiblemove)
			grid, possiblemove = RefGrid(grid, possiblemove, int(move), pc)
			if CheckAll(grid, pc):
				Pgrid(grid)
				print("You lost! Now the computer starts first.")
				win = pc
				turn_player, turn_pc = 0, 1 #### PC starts 1st, player - 2nd
				break	
	if win == "":
		Pgrid(grid)
		print("Nobody is the winner")
	if not PlayAgain():
		break
	elif win == "":
		turn_player, turn_pc = FirstMove()	
############## End Game ########################################
