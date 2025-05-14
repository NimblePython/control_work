# **Контрольная работа по ООП (C++)**  
## **Тема: Реализация игры "Четыре в ряд" (Connect Four)**  

### **1. Цель задания**  
Разработать консольную игру "Четыре в ряд" на C++ с использованием принципов объектно-ориентированного программирования (ООП).  

---

### **2. Требования к реализации**  
#### **Обязательные классы**  
1. **`Grid` (Игровое поле)**  
   - Хранит состояние сетки (пусто/игрок 1/игрок 2) в виде двумерного массива.  
   - Методы:  
     - `placePiece()` — добавление фишки в столбец.  
     - `checkWin()` — проверка победы (4+ фишек в ряд).  
     - `initGrid()` — сброс поля перед новой партией.
   - Вариант структуры:
```cpp
    class Grid {
    private:
        int rows;
        int columns;
        vector<vector<int>> grid;
    public:
        Grid(int rows, int columns);
        void initGrid();
        vector<vector<int>> getGrid();
        int getColumnCount();
        int placePiece(int column, ??? piece); // переменная типа enum
        bool checkWin(int connectN, int row, int col, ??? piece); // переменная типа enum
    }; 
```

2. **`Player` (Игрок)**  
   - Хранит имя и цвет фишек (например, `YELLOW` и `RED`).  
   - Вариант структуры:
```cpp
    class Player {
    private:
        string name;
        ??? piece; // переменная типа enum

    public:
        Player(string name, ??? piece);
        string getName();
        ??? getPieceColor();
    };
```

3. **`Game` (Игровой процесс)**  
   - Управляет очередностью ходов, подсчётом очков, выводом поля в консоль.  
   - Методы:  
     - `playMove()` — обработка хода игрока.  
     - `playRound()` — проведение одной партии.  
     - `play()` — запуск игры до достижения целевого счёта. 
```cpp
    class Game {
    private:
        Grid* grid;
        int connectN;
        vector<Player*> players;
        unordered_map<string, int> score; // 
        int targetScore;

    public:
        Game(Grid* grid, int connectN, int targetScore);

        void printBoard();
        vector<int> playMove(Player* player) {
            // логика хода игрока
        }

        Player* playRound() {
            while (true) {
                for (Player* player : this->players) {
                    vector<int> pos = playMove(player);
                    int row = pos[0];
                    int col = pos[1];
                    GridPosition pieceColor = player->getPieceColor();
                    if (this->grid->checkWin(this->connectN, row, col, pieceColor)) {
                        this->score[player->getName()] = this->score[player->getName()] + 1;
                        return player;
                    }
                }
            }
        }

        void play() {
            int maxScore = 0;
            Player* winner = nullptr;
            while (maxScore < this->targetScore) {
                winner = playRound();
                cout << winner->getName() << " выиграл раунд" << endl;
                maxScore = max(this->score[winner->getName()], maxScore);

                this->grid->initGrid(); // инициализация поля
            } 
            cout << winner->getName() << " выиграл игру" << endl;
        }
    };
```
    

#### **Дополнительные требования**  
- Использовать `enum` для представления состояния клетки (`EMPTY`, `YELLOW`, `RED`).  
- Реализовать проверку победы по **горизонтали, вертикали и двум диагоналям**.  
- Поддержка **настраиваемого размера поля** (например, 6×7, 8×10).  
- Игра ведётся до достижения **N побед** (например, до 3 выигранных партий).  
- Использовать main():
```cpp
    int main() {
        Grid* grid = new Grid(6, 7);
        Game* game = new Game(grid, 4, 10);
        game->play();
        return 0;
    }
```

---

### **3. Пример работы программы**  
```plaintext
Board:
0 0 0 0 0 0 0
0 0 0 0 0 0 0
0 0 0 Y 0 0 0
0 0 R Y R 0 0
0 R Y R Y 0 0
R Y R Y R Y 0

Ход игрока RED. Введите столбец (0-6): 3
Игрок RED выиграл партию!
Текущий счёт: RED - 1, YELLOW - 0
```

---

### **4. Критерии оценки**  
| **Балл** | **Требование** |
|----------|----------------|
| **5**    | Корректная работа базовой логики (ходы, победа). Использование предлагаемого main() |  
| **3**    | Настраиваемый размер поля и длина ряда для победы. |  
| **2**    | Чистый код с соблюдением принципов ООП. Многофайловый проект |  


---

### **5. Рекомендации**  
- Для ввода данных использовать `std::cin`.  
- Для вывода поля — циклы с форматированием.  
- Проверку победы вынести в отдельный метод `checkWin()`.  

**Срок сдачи:** 45 минут
**Формат сдачи:** Исходный код (.cpp/.h) + краткий отчёт (1-2 стр.) с описанием архитектуры.  

--- 

**Удачи в выполнении!**
