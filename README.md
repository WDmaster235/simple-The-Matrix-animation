# simple-The-Matrix-animation
- A simple C# animation representing the famous animation from the matrix -

# Matrix Effect Animation

This project provides a console-based "Matrix" rain effect animation where characters fall in a column-like manner across the screen. The animation displays characters in green, reminiscent of the iconic Matrix digital rain.

## Features

- Displays falling characters in a matrix-like animation.
- All drops have the same color (green).
- Smooth animation with customizable screen size.
- Full-screen support for console environments.

## Usage

To run the `MatrixAnimation` class, you need to invoke the `StartAnimation` method, which will display the Matrix effect using green-colored characters. The animation runs indefinitely until you stop the program manually.

### Prerequisites

- .NET runtime or development environment
- Console or terminal window

### Getting Started

1. **Clone the repository**:

   ```bash
   git clone https://github.com/yourusername/matrix-effect-animation.git
   ```
2. **Open the project in your preferred development environment (e.g., Visual Studio, Visual Studio Code)**.

3. **Run the program by executing the MatrixAnimation.StartAnimation() method**.


**Example Code**
Here is an example of how to run the animation:

```cs
using System;

namespace MatrixEffect
{
    class Program
    {
        static void Main(string[] args)
        {
            // Start the matrix animation with green-colored characters
            MatrixAnimation.StartAnimation();
        }
    }
}
```
**MatrixAnimation Class**
The MatrixAnimation class contains a static method StartAnimation() that runs the green Matrix rain effect in the console.

```cs
public static class MatrixAnimation
{
    private static Random random = new Random();

    public static void StartAnimation()
    {
        Console.CursorVisible = false;
        SetFullScreen();

        int width = Console.WindowWidth;
        int height = Console.WindowHeight;

        StringBuilder[] screenBuffer = new StringBuilder[height];
        for (int i = 0; i < height; i++)
        {
            screenBuffer[i] = new StringBuilder(new string(' ', width));
        }

        int[] drops = new int[width];
        bool[] activeDrops = new bool[width];

        for (int i = 0; i < width; i++)
        {
            drops[i] = random.Next(height);
        }

        int trailLength = 6;
        int maxDrops = 20;

        while (true)
        {
            for (int i = 0; i < height; i++)
            {
                screenBuffer[i].Clear();
                screenBuffer[i].Append(new string(' ', width)); // Reset to empty spaces
            }

            int activeDropCount = 0;

            for (int i = 0; i < width; i++)
            {
                if (drops[i] < height && !activeDrops[i])
                {
                    activeDrops[i] = true;
                }

                if (activeDrops[i])
                {
                    int dropPosition = drops[i];

                    // Draw the current drop character with green color
                    screenBuffer[dropPosition][i] = (char)random.Next(33, 127);

                    // Draw the trailing characters
                    for (int j = 1; j <= trailLength; j++)
                    {
                        int trailPosition = (dropPosition - j + height) % height;
                        screenBuffer[trailPosition][i] = (char)random.Next(33, 127);
                    }

                    drops[i] = (drops[i] + 1) % height;

                    if (drops[i] == 0)
                    {
                        activeDrops[i] = false;
                    }

                    activeDropCount++;
                }
            }

            if (activeDropCount < maxDrops)
            {
                int newDropIndex;
                do
                {
                    newDropIndex = random.Next(width);
                }
                while (activeDrops[newDropIndex]);

                activeDrops[newDropIndex] = true;
                drops[newDropIndex] = 0;
            }

            PrintScreen(width, height, screenBuffer); // Pass the buffer for printing

            Thread.Sleep(30);
        }
    }

    private static void PrintScreen(int width, int height, StringBuilder[] screenBuffer)
    {
        for (int i = 0; i < height; i++)
        {
            Console.SetCursorPosition(0, i);
            Console.ForegroundColor = ConsoleColor.Green; // Set color to green for all drops
            Console.WriteLine(screenBuffer[i]);
        }
    }

    private static void SetFullScreen()
    {
        try
        {
            Console.SetWindowSize(Console.LargestWindowWidth, Console.LargestWindowHeight);
            Console.SetBufferSize(Console.LargestWindowWidth, Console.LargestWindowHeight);
        }
        catch (ArgumentOutOfRangeException)
        {
            Console.WriteLine("Full-screen mode is not supported in this environment.");
        }
    }
}
```

## How the Animation Works

- **Drops**: 
  - Characters fall in columns, with each column having its own individual drop. The drops continuously cascade from top to bottom.

- **Trail**: 
  - As each drop falls, it leaves a trail of characters behind. This trail gradually fades as the characters move further down the screen, creating the iconic Matrix rain effect.

- **Full-Screen**: 
  - The animation adjusts automatically to the **largest available console window size**, maximizing the screen for the best visual experience. If full-screen mode is unsupported, the program defaults to a standard size.

- **Character Randomization**: 
  - The falling characters are **randomly selected** from a set of printable characters, similar to the original Matrix digital rain effect, where each character can be a different symbol, letter, or number.

---

## Troubleshooting

- **Full-Screen Mode**: 
  - If the console does not support full-screen mode, the program will revert to the **default window size**. A message will inform you of this limitation, and the animation will still run.

- **Speed Adjustment**: 
  - If the animation feels too fast or too slow, you can modify the **speed of the drops** by adjusting the `Thread.Sleep(30)` value in the `StartAnimation` method. Increasing the value will slow the drops, and decreasing it will make them fall faster.

---

## Contributing

Feel free to **fork** the repository and create a **pull request** if you have any suggestions or improvements. Contributions are welcome to help enhance this project!
