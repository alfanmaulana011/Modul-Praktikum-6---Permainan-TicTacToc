# Modul-Praktikum-6-Permainan-TicTacToe
## Alfan Maulana
## 5311421011
## Teknik Elektro

### Code State.java:
/** * Enumeration for the various states of the game */ public enum
State { // to save as "GameState.java"
PLAYING, DRAW, CROSS_WON, NOUGHT_WON
}

Penjelasan Program:
Program diatas mendefinisikan 4 kemungkinan yang akan terjadi dalam permainan tictactoe, dianataranya:
1. PLAYING: Mendefinisikan permainan sedang berlangsung
2. DRAW: Kondisi ini akan terjadi ketika semua sel di papan permainan telah diisi, tetapi tidak ada pemenang dalam permainan tictactoe sehingga permainan dianggap seri atau imbang
3. CROSS_WON: Mendefinisikan ketika pemain dengan simbol silang "X" memenangkan permainan tictactoe dengan menyusun tiga tanda "X" secara horizontal, vertikal, atau diagonal
4. NOUGHT_WON: Mendefinisikan ketika pemain dengan simbol lingkaran "O" memenangkan permainan tictactoe dengan menyusun tiga tanda "O" secara horizontal, vertikal, atau diagonal


### Code Seed.java:
/** * Enumeration for the seeds and cell contents */ public enum Seed {
// to save as "Seed.java"
        EMPTY, CROSS, NOUGHT
}

Penjelasan Program:

### Code GameState.java:
/** * Enumeration for the various states of the game */ public enum
GameState { // to save as "GameState.java"
        PLAYING, DRAW, CROSS_WON, NOUGHT_WON 
}

Penjelasan Program:

### Code Cell.java:
import java.awt.Graphics;
import java.awt.*;
import java.awt.Graphics2D;
public class Cell {
       //content of this cell (Seed.EMPTY, Seed.CROSS, or Seed.NOUGHT)
       Seed content;
       int row, col; // row and column of this cell

   /**Constructor to initialize this cell with the specified row and col */
   public Cell(int row, int col) {
      this.row = row;
      this.col = col;
      clear(); // clear content
   }
   /** Clear this cell's content to EMPTY */
   public void clear() {
   content = Seed.EMPTY;
   }
   /**Paint itself on the graphics canvas, given the Graphics context */
   public void paint(Graphics g) {
      // Use Graphics2D which allows us to set the pen's stroke
      Graphics2D g2d = (Graphics2D)g;
      g2d.setStroke(new BasicStroke(GameMain.SYMBOL_STROKE_WIDTH,
         BasicStroke.CAP_ROUND, BasicStroke.JOIN_ROUND)); // Graphics2D only
      // Draw the Seed if it is not empty
      int x1 = col * GameMain.CELL_SIZE + GameMain.CELL_PADDING;
      int y1 = row * GameMain.CELL_SIZE + GameMain.CELL_PADDING;
      if (content == Seed.CROSS) {
         g2d.setColor(Color.RED);
         int x2 = (col + 1) * GameMain.CELL_SIZE - GameMain.CELL_PADDING;
         int y2 = (row + 1) * GameMain.CELL_SIZE - GameMain.CELL_PADDING;
         g2d.drawLine(x1, y1, x2, y2);
         g2d.drawLine(x2, y1, x1, y2);
      } else if (content == Seed.NOUGHT) {
         g2d.setColor(Color.BLUE);
         g2d.drawOval(x1, y1, GameMain.SYMBOL_SIZE, GameMain.SYMBOL_SIZE);
      }
   }
}

Penjelasan Program:

### Board.java:
import java.awt.*;
/**
* The Board class models the ROWS-by-COLS game-board.
*/
public class Board {
   // package access
   // composes of 2D array of ROWS-by-COLS Cell instances
    Cell[][] cells;
    
    /** Constructor to initialize the game board */
    public Board() {
       
       // allocate the array
       cells = new Cell[GameMain.ROWS][GameMain.COLS];
       for (int row = 0; row < GameMain.ROWS; ++row) {
          for (int col = 0; col < GameMain.COLS; ++col) {
            // allocate element of array
             cells[row][col] = new Cell(row, col);
          }
      }
    }
    /** Initialize (or re-initialize) the game board */
    public void init() {
       for (int row = 0; row < GameMain.ROWS; ++row) {
          for (int col = 0; col < GameMain.COLS; ++col) {
             // clear the cell content
             cells[row][col].clear();
          }
       }
    }
    /** Return true if it is a draw (i.e., no more EMPTY cell) */
    public boolean isDraw() {
       for (int row = 0; row < GameMain.ROWS; ++row) {
          for (int col = 0; col < GameMain.COLS; ++col) {
             if (cells[row][col].content == Seed.EMPTY) {
               // an empty seed found, not a draw, exit
                return false;
             }
          }
       }
       return true; // no empty cell, it's a draw
    }
    
    /** Return true if the player with "seed" has won after placing at
       (seedRow, seedCol) */
    public boolean hasWon(Seed seed, int seedRow, int seedCol) {
       return (cells[seedRow][0].content == seed // 3-in-the-row
                 && cells[seedRow][1].content == seed
                 && cells[seedRow][2].content == seed
             || cells[0][seedCol].content == seed // 3-in-the-column
                 && cells[1][seedCol].content == seed
                 && cells[2][seedCol].content == seed
             || seedRow == seedCol // 3-in-the-diagonal
                 && cells[0][0].content == seed
                 && cells[1][1].content == seed
                 && cells[2][2].content == seed
             || seedRow + seedCol == 2 // 3-in-the-opposite-diagonal
                 && cells[0][2].content == seed
                 && cells[1][1].content == seed
                 && cells[2][0].content == seed);
    }
    /** Paint itself on the graphics canvas, given the Graphics context */
    public void paint(Graphics g) {
       // Draw the grid-lines
       g.setColor(Color.GRAY);
       for (int row = 1; row < GameMain.ROWS; ++row) {
          g.fillRoundRect(0, GameMain.CELL_SIZE * row -
 GameMain.GRID_WIDHT_HALF,
            GameMain.CANVAS_WIDTH-1, GameMain.GRID_WIDTH,
     GameMain.GRID_WIDTH, GameMain.GRID_WIDTH);
       }
       for (int col = 1; col < GameMain.COLS; ++col) {
          g.fillRoundRect(GameMain.CELL_SIZE * col -
 GameMain.GRID_WIDHT_HALF, 0,
       GameMain.GRID_WIDTH, GameMain.CANVAS_HEIGHT - 1,
     GameMain.GRID_WIDTH, GameMain.GRID_WIDTH);
       }
       // Draw all the cells
       for (int row = 0; row < GameMain.ROWS; ++row) {
          for (int col = 0; col < GameMain.COLS; ++col) {
             cells[row][col].paint(g); // ask the cell to paint itself
          }
       }
    }
 }

Penjelasan Program:

### Code GameMain.java:
import java.awt.*;
import java.awt.event.*;
import javax.swing.*; 
/**
 * Tic-Tac-Toe: Two-player Graphic version with better OO design.
 * The Board and Cell classes are separated in their own classes.
 */ 
@SuppressWarnings("serial")
public class GameMain extends JPanel { 
   // Named-constants for the game board 
   public static final int ROWS = 3;  // ROWS by COLS cells
   public static final int COLS = 3; 
   public static final String TITLE = "Tic Tac Toe";
  
   // Name-constants for the various dimensions used for graphics drawing 
   public static final int CELL_SIZE = 100; // cell width and height (square)
   public static final int CANVAS_WIDTH = CELL_SIZE * COLS;  // the drawing canvas
   public static final int CANVAS_HEIGHT = CELL_SIZE * ROWS; 
   public static final int GRID_WIDTH = 8;  // Grid-line's width 
   public static final int GRID_WIDHT_HALF = GRID_WIDTH / 2; // Grid-line's half-width 
   // Symbols (cross/nought) are displayed inside a cell, with padding from border 
   
   public static final int CELL_PADDING = CELL_SIZE / 6;
   public static final int SYMBOL_SIZE = CELL_SIZE - CELL_PADDING * 2;
   public static final int SYMBOL_STROKE_WIDTH = 8; // pen's stroke width
 
   private Board board;            // the game board
   private GameState currentState;//the current state of the game
   private Seed currentPlayer;     // the current player
   private JLabel statusBar;     // for displaying status message
  
   /** Constructor to setup the UI and game components */ 
   public GameMain() {
  
      // This JPanel fires MouseEvent 
      this.addMouseListener(new MouseAdapter() {
         @Override
         public void mouseClicked(MouseEvent e) {  // mouse-clicked handler
            int mouseX = e.getX();
            int mouseY = e.getY(); 
            // Get the row and column clicked 
            int rowSelected = mouseY / CELL_SIZE;
            int colSelected = mouseX / CELL_SIZE;
 
            if (currentState == GameState.PLAYING) {
               if (rowSelected >= 0 && rowSelected < ROWS
                     && colSelected >= 0 && colSelected < COLS
                     && board.cells[rowSelected][colSelected].content == Seed.EMPTY) {
                  board.cells[rowSelected][colSelected].content = currentPlayer; // move
                  updateGame(currentPlayer, rowSelected, colSelected); // update currentState
                  // Switch player 
                  currentPlayer = (currentPlayer == Seed.CROSS) ? Seed.NOUGHT : Seed.CROSS;
               }
            } else {        // game over
               initGame();  // restart the game
            } 
            // Refresh the drawing canvas 
            repaint();  // Call-back paintComponent().
         }
      });
  
      // Setup the status bar (JLabel) to display status message 
      statusBar = new JLabel("         ");
      statusBar.setFont(new Font(Font.DIALOG_INPUT, Font.BOLD, 14));
      statusBar.setBorder(BorderFactory.createEmptyBorder(2, 5, 4, 5));
      statusBar.setOpaque(true);
      statusBar.setBackground(Color.LIGHT_GRAY);
 
      setLayout(new BorderLayout()); 
      add(statusBar, BorderLayout.PAGE_END); // same as SOUTH
      setPreferredSize(new Dimension(CANVAS_WIDTH, CANVAS_HEIGHT + 30)); 
            // account for statusBar in height 
 
      board = new Board();   // allocate the game-board
      initGame();  // Initialize the game variables
   }
  
   /** Initialize the game-board contents and the current-state */ 
   public void initGame() {
      for (int row = 0; row < ROWS; ++row) {
         for (int col = 0; col < COLS; ++col) {
            board.cells[row][col].content = Seed.EMPTY; // all cells empty
         }
      }
      currentState = GameState.PLAYING;  // ready to play
      currentPlayer = Seed.CROSS;        // cross plays first
   }
  
   /** Update the currentState after the player with "theSeed" has placed on (row, col) */ 
   public void updateGame(Seed theSeed, int row, int col) {
      if (board.hasWon(theSeed, row, col)) {  // check for win
         currentState = (theSeed == Seed.CROSS) ? GameState.CROSS_WON : GameState.NOUGHT_WON;
      } else if (board.isDraw()) {  // check for draw
         currentState = GameState.DRAW;
      } 
      // Otherwise, no change to current state (PLAYING). 
   }
  
   /** Custom painting codes on this JPanel */ 
   @Override
   public void paintComponent(Graphics g){ 
      //invoke via repaint()
      super.paintComponent(g);    // fill background 
      setBackground(Color.WHITE); // set its background color
 
      board.paint(g);  // ask the game board to paint itself 
 
      // Print status-bar message 
      if (currentState == GameState.PLAYING) {
         statusBar.setForeground(Color.BLACK);
         if (currentPlayer == Seed.CROSS) {
            statusBar.setText("X's Turn");
         } else {
            statusBar.setText("O's Turn");
         }
      } else if (currentState == GameState.DRAW) {
         statusBar.setForeground(Color.RED);
         statusBar.setText("It's a Draw! Click to play again.");
      } else if (currentState == GameState.CROSS_WON) {
         statusBar.setForeground(Color.RED);
         statusBar.setText("'X' Won! Click to play again.");
      } else if (currentState == GameState.NOUGHT_WON) {
         statusBar.setForeground(Color.RED);
         statusBar.setText("'O' Won! Click to play again.");
      }
   }
  
   /** The entry "main" method */ 
   public static void main(String[] args) {
      // Run GUI construction codes in Event-Dispatching thread for thread safety 
      javax.swing.SwingUtilities.invokeLater(new Runnable() { 
         public void run() {
            JFrame frame = new JFrame(TITLE); 
     // Set the content-pane of the JFrame to an instance of main JPanel 
            frame.setContentPane(new GameMain());
            frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
            frame.pack(); 
            // center the application window
            frame.setLocationRelativeTo(null);  
     frame.setVisible(true);   // show it
         }
      });
   }
}

Penjelasan Program:
