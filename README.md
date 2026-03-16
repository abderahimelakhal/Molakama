I'll help you find information about creating a simple Android puzzle game in Java.Great! Here's a **complete guide for creating a simple Android puzzle game in Java**:

---

## **Project Overview: Sliding Puzzle Game (8-Puzzle)**

This is a beginner-friendly sliding puzzle where players arrange numbered tiles in order by sliding them into an empty space.

---

## **Step 1: Create a New Android Project**

- Open **Android Studio** → **File → New → New Project**
- Select **Empty Activity** template
- Language: **Java**
- Minimum SDK: API 21 or higher

---

## **Step 2: Create the Layout**

````xml name=res/layout/activity_main.xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp"
    tools:context=".MainActivity">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Sliding Puzzle Game"
        android:textSize="24sp"
        android:textStyle="bold"
        android:layout_marginBottom="16dp" />

    <GridLayout
        android:id="@+id/gridLayout"
        android:layout_width="300dp"
        android:layout_height="300dp"
        android:rowCount="3"
        android:columnCount="3"
        android:layout_gravity="center" />

    <Button
        android:id="@+id/resetButton"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Reset Game"
        android:layout_marginTop="16dp"
        android:layout_gravity="center" />

</LinearLayout>
````

---

## **Step 3: Create the Main Activity**

```java name=MainActivity.java
package com.example.puzzlegame;

import android.os.Bundle;
import android.view.ViewGroup;
import android.widget.Button;
import android.widget.GridLayout;
import androidx.appcompat.app.AppCompatActivity;
import java.util.ArrayList;
import java.util.Collections;

public class MainActivity extends AppCompatActivity {

    private ArrayList<Integer> tiles;
    private Button[][] buttons;
    private GridLayout gridLayout;
    private static final int GRID_SIZE = 3;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        gridLayout = findViewById(R.id.gridLayout);
        Button resetButton = findViewById(R.id.resetButton);

        buttons = new Button[GRID_SIZE][GRID_SIZE];
        tiles = new ArrayList<>();

        // Initialize the game
        initializeTiles();
        createTileButtons();

        resetButton.setOnClickListener(v -> resetGame());
    }

    private void initializeTiles() {
        tiles.clear();
        // Add numbers 1-8 and 0 (for empty space)
        for (int i = 1; i < GRID_SIZE * GRID_SIZE; i++) {
            tiles.add(i);
        }
        tiles.add(0); // 0 represents the empty space
        Collections.shuffle(tiles);
    }

    private void createTileButtons() {
        gridLayout.removeAllViews();
        int index = 0;

        for (int row = 0; row < GRID_SIZE; row++) {
            for (int col = 0; col < GRID_SIZE; col++) {
                final int currentRow = row;
                final int currentCol = col;
                final int tileValue = tiles.get(index);

                Button button = new Button(this);
                button.setLayoutParams(new GridLayout.LayoutParams(
                        GridLayout.spec(row, 1, 1f),
                        GridLayout.spec(col, 1, 1f)
                ));

                if (tileValue == 0) {
                    button.setText("");
                    button.setBackgroundColor(0xFFDDDDDD); // Light gray for empty
                } else {
                    button.setText(String.valueOf(tileValue));
                    button.setTextSize(20);
                    button.setBackgroundColor(0xFF4CAF50); // Green color
                }

                button.setOnClickListener(v -> moveTile(currentRow, currentCol));
                buttons[row][col] = button;
                gridLayout.addView(button);
                index++;
            }
        }
    }

    private void moveTile(int row, int col) {
        // Find the empty space
        int emptyRow = -1, emptyCol = -1;

        for (int i = 0; i < GRID_SIZE; i++) {
            for (int j = 0; j < GRID_SIZE; j++) {
                if (buttons[i][j].getText().equals("")) {
                    emptyRow = i;
                    emptyCol = j;
                }
            }
        }

        // Check if the clicked tile is adjacent to the empty space
        if (isAdjacent(row, col, emptyRow, emptyCol)) {
            // Swap the tiles
            String temp = buttons[row][col].getText().toString();
            buttons[row][col].setText(buttons[emptyRow][emptyCol].getText());
            buttons[emptyRow][emptyCol].setText(temp);

            // Check if the puzzle is solved
            if (isPuzzleSolved()) {
                showWinMessage();
            }
        }
    }

    private boolean isAdjacent(int row1, int col1, int row2, int col2) {
        return (Math.abs(row1 - row2) + Math.abs(col1 - col2)) == 1;
    }

    private boolean isPuzzleSolved() {
        int expectedValue = 1;
        for (int i = 0; i < GRID_SIZE; i++) {
            for (int j = 0; j < GRID_SIZE; j++) {
                String buttonText = buttons[i][j].getText().toString();
                if (buttonText.isEmpty()) {
                    return expectedValue == GRID_SIZE * GRID_SIZE;
                }
                if (Integer.parseInt(buttonText) != expectedValue) {
                    return false;
                }
                expectedValue++;
            }
        }
        return true;
    }

    private void showWinMessage() {
        android.widget.Toast.makeText(this, "🎉 You Won!", android.widget.Toast.LENGTH_LONG).show();
    }

    private void resetGame() {
        initializeTiles();
        createTileButtons();
    }
}
```

---

## **Step 4: Update AndroidManifest.xml (if needed)**

The manifest should already have the main activity registered. Just verify it looks like:

```xml name=AndroidManifest.xml
<activity
    android:name=".MainActivity"
    android:exported="true">
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
</activity>
```

---

## **How to Run:**

1. Click **Run** (Shift + F10) in Android Studio
2. Select an emulator or connected device
3. The game will launch and you can start playing!

---

## **Features Included:**

✅ 3×3 grid layout  
✅ Numbered tiles (1-8) with one empty space  
✅ Shuffle on game start  
✅ Click tiles to move them  
✅ Win detection with toast message  
✅ Reset button to restart the game  

---

## **Possible Enhancements:**

- Add move counter
- Add timer
- Add difficulty levels (4×4 grid)
- Add animations
- Save high scores
- Add sound effects

Would you like me to help you expand this with any of these features?
