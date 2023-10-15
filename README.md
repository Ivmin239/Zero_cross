import numpy as np
#---крестики-нолики---

#функция отображения доски
def boardvis(board):
  print("  {}  | {} |  {}  ".format(board[0], board[1], board[2]))
  print("  --------  ")
  print("  {}  | {} |  {}  ".format(board[3], board[4], board[5]))
  print("  --------  ")
  print("  {}  | {} |  {}  ".format(board[6], board[7], board[8]))
  

#функция для выбора кто ходит первым (рандом)
def order():
  flag = 1
  pos = np.random.randint(1, 10, dtype=int)
  if (pos%2==1):
    flag = 0
  return 'X', 'O', flag
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
  if (board[int(pos)-1] != "X")&(board[int(pos)-1] != "O"):
    return True
  else:
    return False
  
#функция, в которой игрок делает ход
#if num in ['1','2','0','4','5','X','7','8','9']:
#num - подходит - заменить на соотв символ
def Actplayer(board, player_sym):
  nump = ''
  while not (checkfree(board, nump)) or not (nump in "123456789"):
    nump = input("Выберите номер клетки от 1 до 9 из не занятых")
  board[int(nump)-1] = player_sym
  
  
#функция проверяющая выграл ли игрок, компьютер или ничья (поле заполнено)
def result(b):
  if ((b[0] == "X")&(b[1] == "X")&(b[2] == "X")) or                       ((b[3] == "X")&(b[4] == "X")&(b[5] == "X")) or                       ((b[6] == "X")&(b[7] == "X")&(b[8] == "X")) or                       ((b[0] == "X")&(b[3] == "X")&(b[6] == "X")) or                       ((b[1] == "X")&(b[4] == "X")&(b[7] == "X")) or                       ((b[2] == "X")&(b[5] == "X")&(b[8] == "X")) or                       ((b[0] == "X")&(b[4] == "X")&(b[8] == "X")) or                       ((b[2] == "X")&(b[4] == "X")&(b[6] == "X")):
    return 1
  elif ((b[0] == "O")&(b[1] == "O")&(b[2] == "O")) or                       ((b[3] == "O")&(b[4] == "O")&(b[5] == "O")) or                       ((b[6] == "O")&(b[7] == "O")&(b[8] == "O")) or                       ((b[0] == "O")&(b[3] == "O")&(b[6] == "O")) or                       ((b[1] == "O")&(b[4] == "O")&(b[7] == "O")) or                       ((b[2] == "O")&(b[5] == "O")&(b[8] == "O")) or                       ((b[0] == "O")&(b[4] == "O")&(b[8] == "O")) or                       ((b[2] == "O")&(b[4] == "O")&(b[6] == "O")):
      return 0
  elif ''.join(b).isalpha():
    return 2
#функция хода компьютера (1 версия - просто рандомный ход на пустое поле)
def Actcomp(board, comp_sym):
  numc = np.random.randint(0, 8, dtype=int)
  while not (checkfree(board, numc)):
    numc = np.random.randint(0, 8, dtype=int)
  board[numc] = comp_sym
'''

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
      Actcomp(board, comp_sym)
      boardvis(board)
     
      if result(board) == -1:
        turn = 1
      else:
        resultcheck(board)
        break
  if(again() == 0):
    break
