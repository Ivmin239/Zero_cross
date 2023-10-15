import numpy as np
#---крестики-нолики---

#функция отображения доски
def boardvis(board):
  print("  {}  | {} |  {}  ".format(board[0], board[1], board[2]))
  print("  ------------  ")
  print("  {}  | {} |  {}  ".format(board[3], board[4], board[5]))
  print("  ------------  ")
  print("  {}  | {} |  {}  \n".format(board[6], board[7], board[8]))


#функция для выбора кто ходит первым 
def order():
  flag = 1
  while True:
    sym = int(input(Выберите свой символ: 1 - "X", 2 - "O"))
    if (sym == 1) or (sym ==2):
       break
  if (sym == 1):
    pos = np.random.randint(1, 10, dtype=int)
    if (pos%2==1):
      flag = 0
    return 'X', 'O', flag
  elif(order == 2):
    pos = np.random.randint(1, 10, dtype=int)
    if (pos%2==1):
      flag = 0
    return 'O', 'X', flag


#функция для предложения сыграть еще
def again():
  while True:
    try:
      answer = int(input("Начать заново? (1 - да, 0 - нет)"))
      return answer
    except:
      print('Вы ошиблись в вводе')

#функция проверяющая занята клетка или нет
def checkfree(board, pos):
  if (board[int(pos)-1] != 'X')&(board[int(pos)-1] != 'O'):
    return True
  else:
    return False

#функция, в которой игрок делает ход
#if num in ['1','2','0','4','5','X','7','8','9']:
#num - подходит - заменить на соотв символ
def Actplayer(board, player_sym):
  nump = '!'
  #debug
  #print('I am here in function actplayer')
  #print(nump in "123456789")
  while not (nump in "123456789"):
    nump = input("Выберите номер клетки от 1 до 9 из не занятых: ")
    while not (checkfree(board, nump)):
      nump = input("Клетка занята! \nВыберите номер клетки от 1 до 9 из не занятых: ")
  board[int(nump)-1] = player_sym


#функция проверяющая выграл ли игрок, компьютер или ничья (поле заполнено)
def result(b):
  if ((b[0] == "player_sym")&(b[1] == "player_sym")&(b[2] == "player_sym")) or                       ((b[3] == "player_sym")&(b[4] == "player_sym")&(b[5] == "player_sym")) or                       ((b[6] == "player_sym")&(b[7] == "player_sym")&(b[8] == "player_sym")) or                       ((b[0] == "player_sym")&(b[3] == "player_sym")&(b[6] == "player_sym")) or                       ((b[1] == "player_sym")&(b[4] == "player_sym")&(b[7] == "player_sym")) or                       ((b[2] == "player_sym")&(b[5] == "player_sym")&(b[8] == "player_sym")) or                       ((b[0] == "player_sym")&(b[4] == "player_sym")&(b[8] == "player_sym")) or                       ((b[2] == "player_sym")&(b[4] == "player_sym")&(b[6] == "player_sym")):
    return 1
  elif ((b[0] == "comp_sym")&(b[1] == "comp_sym")&(b[2] == "comp_sym")) or                       ((b[3] == "comp_sym")&(b[4] == "comp_sym")&(b[5] == "comp_sym")) or                       ((b[6] == "comp_sym")&(b[7] == "comp_sym")&(b[8] == "comp_sym")) or                       ((b[0] == "comp_sym")&(b[3] == "comp_sym")&(b[6] == "comp_sym")) or                       ((b[1] == "comp_sym")&(b[4] == "comp_sym")&(b[7] == "comp_sym")) or                       ((b[2] == "comp_sym")&(b[5] == "comp_sym")&(b[8] == "comp_sym")) or                       ((b[0] == "comp_sym")&(b[4] == "comp_sym")&(b[8] == "comp_sym")) or                       ((b[2] == "comp_sym")&(b[4] == "comp_sym")&(b[6] == "comp_sym")):
      return 0
  elif ''.join(b).isalpha():
    return 2
  else:
    return -1
#функция хода компьютера (1 версия - просто рандомный ход на пустое поле)
def Actcomp(board, comp_sym):
  numc = np.random.randint(1, 9, dtype=int)
  #debug
  #print("ход компьютера: ", numc, ' is free?: ', checkfree(board, numc))
  while not (checkfree(board, numc)):
    numc = np.random.randint(1, 9, dtype=int)
    #print("ход компьютера: ", numc, ' is free?: ', checkfree(board, numc))
  print("Ход компьютера: ", numc)
  board[numc-1] = comp_sym


def Actcomp_upgr(board, comp_sym):
  if comp_sym == "X":
    player_sym = "O"
  else:
    player_sym = "X"

  for i in range(1,10):
    b_copy = board.copy()
    if checkfree(board, i):
      b_copy[i-1] = comp_sym
      if result(b_copy) == 0: #computer
        print("Ход компьютера: ", i, ' comp win')
        board[i-1] = comp_sym
        return

  for i in range(1,10):
    b_copy = board.copy()
    if checkfree(board, i):
      b_copy[i-1] = player_sym
      if result(b_copy) == 1: #player
        print("Ход компьютера: ", i, ' block player')
        board[i-1] = comp_sym
        return    

  if (checkfree(board, 5 )):
    print("Ход компьютера: ", 5, ' centrer')
    board[4] = comp_sym
    return
  for i in [1, 3, 7, 9]: 
    if (checkfree(board, i)):
      print("Ход компьютера: ", i, ' in corner')
      board[i-1] = comp_sym
      return
  for i in [2, 4, 6, 8]: 
    if (checkfree(board, i)):
      print("Ход компьютера: ", i, ' in side')
      board[i-1] = comp_sym
      return



def resultcheck(board):
  match result(board):
    case 1:
      print("Поздравляем! Вы выиграли!")
    case 0:
      print("К сожалению, Вы проиграли!")
    case 2:
      print("Ничья! Советуем сыграть снова.")
#основная программа

print("Сыграть в крестики-нолики?")
while True:
  #задать начальные параметры символов игры
  board = ['1','2','3','4','5','6','7','8','9']
  player_sym, comp_sym, turn = order()

  print("Символ игрока - ", player_sym)
  if (turn == 1):
    print("Первым ходитите Вы")  
  else:
    print("Первым ходит Компьютер")
  boardvis(board)
  while True:
    if (turn == 1):
      Actplayer(board, player_sym)
      boardvis(board)
      if (result(board)==-1):
        turn = 0
      else:
        resultcheck(board)
        break
    else:
      Actcomp_upgr(board, comp_sym)
      boardvis(board)
      #debug
      #print("\n", result(board), type(result(board)), "\n")
      if result(board) == -1:
        turn = 1
      else:
        resultcheck(board)
        break
  if(again() == 0):
    break
