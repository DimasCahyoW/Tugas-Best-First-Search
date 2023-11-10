# Tugas-Best-First-Search
# Nama : Dimas Cahyo Wicaksono
# NIM  : 5311421093

# Praktikum Best First Search (Tic Tac Toe)

package TicTacToe;
/** * Enumeration for the various states of the game */ public enum
GameState { // to save as "GameState.java"
PLAYING, DRAW, CROSS_WON, NOUGHT_WON
}
package TicTacToe;
/** * Enumeration for the seeds and cell contents */ public enum Seed {
// to save as "Seed.java"
EMPTY, CROSS, NOUGHT
}
package TicTacToe;
/** * Enumeration for the various states of the game */ public enum
State { // to save as "GameState.java"
PLAYING, DRAW, CROSS_WON, NOUGHT_WON
}
Kode di atas merupakan bagian dari implementasi permainan Tic-Tac-Toe dalam bahasa pemrograman Java
File enum GameState yang merepresentasikan berbagai kondisi atau status yang mungkin terjadi dalam permainan Tic-Tac-Toe. Kondisi-kondisi ini mencakup permainan sedang berlangsung (PLAYING), hasil seri (DRAW), serta kemenangan oleh pemain yang menggunakan 'X' (CROSS_WON) atau 'O' (NOUGHT_WON).
File enum Seed yang merepresentasikan isi sel atau petak dalam papan permainan Tic-Tac-Toe. Ada tiga kemungkinan isi: kosong (EMPTY), 'X' (CROSS), atau 'O' (NOUGHT).
File enum State yang juga merepresentasikan berbagai kondisi atau status yang mungkin terjadi dalam permainan Tic-Tac-Toe. Kondisi-kondisi ini mirip dengan yang ada pada GameState.

# Class Cell.java
package TicTacToe;
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
 BasicStroke.CAP_ROUND, BasicStroke.JOIN_ROUND)); // Graphics2D
only
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

1.	Variabel dan Konstruktor
Variabel content merepresentasikan isi dari sel tersebut, yang dapat berupa Seed.EMPTY, Seed.CROSS, atau Seed.NOUGHT. Variabel row dan col menyimpan informasi tentang baris dan kolom sel tersebut di papan. Konstruktor Cell(int row, int col) digunakan untuk menginisialisasi objek Cell dengan baris dan kolom yang sesuai dan kemudian memanggil metode clear() untuk mengosongkan isi sel.
2.	Metode clear()
Metode ini digunakan untuk mengosongkan isi sel dengan mengatur nilai content menjadi Seed.EMPTY.
3.	Metode paint(Graphics g)
Metode ini menggambar sel pada kanvas grafis (Graphics). Itu menggunakan Graphics2D untuk mengatur ketebalan garis dan warna. Selanjutnya, tergantung pada isi sel (content), itu menggambar garis silang (X) berwarna merah atau lingkaran (O) berwarna biru di dalam sel.
•	Jika content adalah Seed.CROSS, maka garis silang merah digambar.
•	Jika content adalah Seed.NOUGHT, maka lingkaran biru digambar.
Koordinat dan ukuran dari garis atau lingkaran dihitung berdasarkan baris dan kolom sel pada papan permainan.

# Class Board.java
package TicTacToe;
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

1.	Konstruktor Board()
Konstruktor ini digunakan untuk menginisialisasi objek Board. Di dalamnya, dilakukan alokasi array dua dimensi dari objek Cell sebanyak ROWS kali COLS. Setiap elemen array diisi dengan objek Cell baru yang sesuai dengan baris dan kolomnya.
2.	Metode init()
Metode init() digunakan untuk menginisialisasi atau mengosongkan kembali papan permainan. Setiap sel diatur ulang ke kondisi awal dengan memanggil metode clear() dari objek Cell.
3.	Metode isDraw() 
Metode isDraw() digunakan untuk mengecek apakah permainan berakhir seri (draw). Jika ada setidaknya satu sel yang masih kosong (Seed.EMPTY), maka permainan belum berakhir seri.
4.	Metode hasWon(Seed seed, int seedRow, int seedCol)
Metode hasWon digunakan untuk mengecek apakah pemain dengan tanda (seed) tertentu (Seed seed) telah memenangkan permainan setelah menempatkan simbol pada sel dengan baris dan kolom tertentu. Metode ini menggunakan logika untuk memeriksa kondisi kemenangan, baik secara horizontal, vertikal, diagonal, atau diagonal terbalik.
5.	Metode paint(Graphics g)
Metode paint(Graphics g) digunakan untuk menggambar papan permainan pada kanvas grafis. Ini termasuk menggambar garis-garis grid dan meminta setiap sel untuk menggambar dirinya sendiri dengan memanggil metode paint(g) dari objek Cell.


# Class GameMain.java
package TicTacToe;
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
 public static final int ROWS = 3; // ROWS by COLS cells
 public static final int COLS = 3;
public static final String TITLE = "Tic Tac Toe";
 // Name-constants for the various dimensions used for graphics drawing
 public static final int CELL_SIZE = 100; // cell width and height (square)
 public static final int CANVAS_WIDTH = CELL_SIZE * COLS; // the drawing
canvas
 public static final int CANVAS_HEIGHT = CELL_SIZE * ROWS;
 public static final int GRID_WIDTH = 8; // Grid-line's width
 public static final int GRID_WIDHT_HALF = GRID_WIDTH / 2; // Grid-line's
half-width
 // Symbols (cross/nought) are displayed inside a cell, with padding from border

 public static final int CELL_PADDING = CELL_SIZE / 6;
 public static final int SYMBOL_SIZE = CELL_SIZE - CELL_PADDING * 2;
 public static final int SYMBOL_STROKE_WIDTH = 8; // pen's stroke width
 private Board board; // the game board
 private GameState currentState;//the current state of the game
 private Seed currentPlayer; // the current player
 private JLabel statusBar; // for displaying status message
 /** Constructor to setup the UI and game components */
 public GameMain() {
 // This JPanel fires MouseEvent
 this.addMouseListener(new MouseAdapter() {
 @Override
 public void mouseClicked(MouseEvent e) { // mouse-clicked handler
 int mouseX = e.getX();
 int mouseY = e.getY();
 // Get the row and column clicked
 int rowSelected = mouseY / CELL_SIZE;
 int colSelected = mouseX / CELL_SIZE;
 if (currentState == GameState.PLAYING) {
 if (rowSelected >= 0 && rowSelected < ROWS
 && colSelected >= 0 && colSelected < COLS
&& board.cells[rowSelected][colSelected].content ==
Seed.EMPTY) {
 board.cells[rowSelected][colSelected].content =
currentPlayer; // move
 updateGame(currentPlayer, rowSelected, colSelected); //
update currentState
 // Switch player
currentPlayer = (currentPlayer == Seed.CROSS) ?
Seed.NOUGHT : Seed.CROSS;
 }
 } else { // game over
 initGame(); // restart the game
 }
 // Refresh the drawing canvas
 repaint(); // Call-back paintComponent().
 }
 });
 // Setup the status bar (JLabel) to display status message
 statusBar = new JLabel(" ");
 statusBar.setFont(new Font(Font.DIALOG_INPUT, Font.BOLD, 14));
 statusBar.setBorder(BorderFactory.createEmptyBorder(2, 5, 4, 5));
 statusBar.setOpaque(true);
 statusBar.setBackground(Color.LIGHT_GRAY);
 setLayout(new BorderLayout());
add(statusBar, BorderLayout.PAGE_END); // same as SOUTH
 setPreferredSize(new Dimension(CANVAS_WIDTH, CANVAS_HEIGHT + 30));
 // account for statusBar in height
 board = new Board(); // allocate the game-board
 initGame(); // Initialize the game variables
 }
 /** Initialize the game-board contents and the current-state */
 public void initGame() {
 for (int row = 0; row < ROWS; ++row) {
 for (int col = 0; col < COLS; ++col) {
 board.cells[row][col].content = Seed.EMPTY; // all cells empty
 }
 }
 currentState = GameState.PLAYING; // ready to play
 currentPlayer = Seed.CROSS; // cross plays first
 }
 /** Update the currentState after the player with "theSeed" has placed on (row, col) */
 public void updateGame(Seed theSeed, int row, int col) {
 if (board.hasWon(theSeed, row, col)) { // check for win
 currentState = (theSeed == Seed.CROSS) ? GameState.CROSS_WON :
GameState.NOUGHT_WON;
 } else if (board.isDraw()) { // check for draw
 currentState = GameState.DRAW;
 }
 // Otherwise, no change to current state (PLAYING).
 }
 /** Custom painting codes on this JPanel */
 @Override
 public void paintComponent(Graphics g){
 //invoke via repaint()
 super.paintComponent(g); // fill background
 setBackground(Color.WHITE); // set its background color
 board.paint(g); // ask the game board to paint itself
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
 frame.setVisible(true); // show it
 }
 });
 }
}

1.	Konstanta dan Dimensi Grafis
Konstanta-konstanta ini mendefinisikan parameter-parameter permainan, seperti jumlah baris dan kolom, ukuran sel, dan konstanta lainnya yang digunakan dalam menggambar papan permainan dan elemen-elemen grafis lainnya.
2.	Variabel dan Objek Kelas
Variabel dan objek yang digunakan dalam kelas GameMain. board merupakan objek dari kelas Board yang merepresentasikan papan permainan. currentState menyimpan status saat ini dari permainan. currentPlayer menyimpan giliran pemain saat ini. statusBar adalah label yang digunakan untuk menampilkan pesan status pada GUI.
3.	Konstruktor GameMain()
Konstruktor ini digunakan untuk mengatur UI dan komponen-komponen permainan. MouseListener ditambahkan untuk menanggapi klik mouse. StatusBar diatur untuk menampilkan pesan status. Objek Board diinisialisasi, dan permainan diinisialisasi melalui metode initGame().
4.	Metode initGame()
Metode ini digunakan untuk menginisialisasi konten papan permainan dan status permainan saat ini. Semua sel diatur menjadi kosong, dan permainan diatur ke status PLAYING dengan pemain CROSS yang bergerak pertama.
5.	Metode updateGame(Seed theSeed, int row, int col)
Metode ini digunakan untuk memperbarui status permainan setelah pemain dengan theSeed telah menempatkan simbolnya di posisi (row, col).
6.	Metode paintComponent(Graphics g)
Metode ini bertanggung jawab untuk menggambar elemen-elemen grafis pada panel. Ini memanggil metode paint(g) dari objek Board untuk menggambar papan permainan dan kemudian menampilkan pesan status pada status bar berdasarkan currentState dan currentPlayer.
7.	Metode main(String[] args)
Metode ini adalah metode utama yang digunakan untuk menjalankan aplikasi. Ini memastikan bahwa kode konstruksi GUI dijalankan dalam Event-Dispatching Thread untuk keamanan thread.

