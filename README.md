# Guessing-number-using-javaFx
import java.util.Random;
import javafx.application.Application;
import javafx.geometry.Insets;
import javafx.geometry.Pos;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.control.Label;
import javafx.scene.control.TextArea;
import javafx.scene.control.TextField;
import javafx.scene.layout.GridPane;
import javafx.scene.layout.HBox;
import javafx.scene.layout.StackPane;
import javafx.stage.Stage;


/**
 * JavaFX App
 */


public class App extends Application {

     private int targetNumber;
    private int maxAttempts = 5;
    private int attemptsLeft;
    private int score;

    private Label feedbackLabel;
    private TextField guessField;
    private Button guessButton;
    private Button playAgainButton;

    @Override
    public void start(Stage primaryStage) {
        primaryStage.setTitle("Number Guessing Game");

        GridPane grid = new GridPane();
        grid.setAlignment(Pos.CENTER);
        grid.setHgap(10);
        grid.setVgap(10);
        grid.setPadding(new Insets(25, 25, 25, 25));

        Label instructionLabel = new Label("Guess the number between 1 and 100:");
        grid.add(instructionLabel, 0, 0, 2, 1);

        guessField = new TextField();
        grid.add(guessField, 0, 1);

        guessButton = new Button("Guess");
        playAgainButton = new Button("Play Again");

        HBox hbBtn = new HBox(10);
        hbBtn.setAlignment(Pos.BOTTOM_CENTER);
        hbBtn.getChildren().addAll(guessButton, playAgainButton);
        grid.add(hbBtn, 0, 2, 2, 1);

        feedbackLabel = new Label();
        grid.add(feedbackLabel, 0, 3, 2, 1);

        initializeGame();

        guessButton.setOnAction(event -> checkGuess());

        playAgainButton.setOnAction(event -> {
            initializeGame();
            feedbackLabel.setText("");
            guessButton.setDisable(false);
            playAgainButton.setDisable(true);
        });

        Scene scene = new Scene(grid, 300, 200);
        primaryStage.setScene(scene);
        primaryStage.show();
    }

    private void initializeGame() {
        Random random = new Random();
        targetNumber = random.nextInt(100) + 1;
        attemptsLeft = maxAttempts;
        score = 0;
    }

    private void checkGuess() {
        int guess;
        try {
            guess = Integer.parseInt(guessField.getText());
        } catch (NumberFormatException e) {
            feedbackLabel.setText("Invalid input. Please enter a number.");
            return;
        }

        attemptsLeft--;

        if (guess == targetNumber) {
            feedbackLabel.setText("Congratulations! You guessed the number!");
            score++;
            guessButton.setDisable(true);
            playAgainButton.setDisable(false);
        } else if (attemptsLeft == 0) {
            feedbackLabel.setText("Sorry, you've run out of attempts. The number was " + targetNumber);
            guessButton.setDisable(true);
            playAgainButton.setDisable(false);
        } else if (guess < targetNumber) {
            feedbackLabel.setText("Too low! Attempts left: " + attemptsLeft);
        } else {
            feedbackLabel.setText("Too high! Attempts left: " + attemptsLeft);
        }

        guessField.clear();
    }

    public static void main(String[] args) {
        launch(args);
    }
}
