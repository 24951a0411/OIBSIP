import java.util.Scanner;

public class NumberGuessingGame {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int maxGuess = 0;
        int range = 100;

        System.out.println("Welcome to NumberGuessing Game!");
        System.out.println("Choose a difficulty level:");
        System.out.println("1. Easy (10 guesses)");
        System.out.println("2. Medium (5 guesses)");
        System.out.println("3. Hard (3 guesses)");
        System.out.print("Enter 1, 2, or 3: ");
        int level = sc.nextInt();

        switch (level) {
            case 1:
                maxGuess = 10;
                break;
            case 2:
                maxGuess = 5;
                break;
            case 3:
                maxGuess = 3;
                break;
            default:
                System.out.println("Invalid input. Defaulting to Medium level.");
                maxGuess = 5;
        }

        int target = 1 + (int)(Math.random() * range);

        boolean guessed = false;
        for (int attempt = 1; attempt <= maxGuess; attempt++) {
            System.out.print("Attempt " + attempt + "/" + maxGuess +
                ": Enter your guess (1-" + range + "): ");
            int guess = sc.nextInt();

            if (guess == target) {
                System.out.println("Congratulations! You guessed the correct number!");
                guessed = true;
                break;
            } else if (guess < target) {
                System.out.println("Your guess is too low!");
            } else {
                System.out.println("Your guess is too high!");
            }
        }

        if (!guessed) {
            System.out.println("Sorry, you're out of guesses. The correct number was " + target + ".");
        }
    }
}
