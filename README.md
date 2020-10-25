# Refactoring

From my PA4 project code: https://github.com/Ing140943/TypingGame

In the `src/application/windows/PlayGame.java`

https://github.com/Ing140943/TypingGame/blob/master/src/application/windows/PlayGame.java

consider this code:

### constructor class
```
public PlayGame(){
        val = 200;
        clock = new TimerByMe(10);
        showUser = "  Your health left " + val + "\n" + "  Your accuracy is "
                + (val/200)*100+ " %" +"\n"
                + "  You are playing Typing Game!"+
                "\n       ........................................" ;
                }
```
### drawWord method
```java
public void drawWord(boolean correct){
       //unrelated code
                else{
                    val -= 5 ;
                    text.setText("  Your health left " + val + "\n" + "  Your accuracy is "
                            + (val/200)*100+ " %" +"\n"
                            + "  You are playing Typing Game!"+
                            "\n       ........................................") ;
        //unrelated code
        }
    }
```
* Problem:
  - We can grouped the code together to one method because drawWord and the constructor have same duplicate code.
* Refactor
  - Move this code to a separate new method and replace the old code with a call to the method.
  
  ```java
  public String textFormat(double val){
        String s = "  Your health left " + val + "\n" + "  Your accuracy is "
                + (val/200)*100+ " %" +"\n"
                + "  You are playing Typing Game!"+
                "\n       ........................................";
        return s;
    }
    
     public PlayGame(){
        val = 200;
        clock = new TimerByMe(10);
        showUser = textFormat(val);
        }
        
       public void checkPosition(boolean correct, int p, Label l) {
        if (p < positionNow){
            l.setTextFill(GREEN);
        }
        if ( p == positionNow){
            if (correct){
                l.setTextFill(BLUE);
            }
            else{
                val -= 5 ;
                text.setText(textFormat(val));
                //unrelated code}
  ```
  
 * Benefits: 
      - The code is more readabl
      - less duplication code.
      - Easier to maintanance the code.
  
In the `src/application/windows/PlayGame.java`

https://github.com/Ing140943/TypingGame/blob/master/src/application/windows/PlayGame.java

consider this code: 
### drawWord method
```java
public void drawWord(boolean correct){
    for (String s : word.split("")){
        Label l = new Label( s );
            if (p < positionNow){
                l.setTextFill(Color.GREEN);
            }
            if ( p == positionNow){
                if (correct){
                    l.setTextFill(Color.BLUE);
                }
                else{
                    val -= 5 ;
                    text.setText("  Your health left " + val + "\n" + "  Your accuracy is "
                            + (val/200)*100+ " %" +"\n"
                            + "  You are playing Typing Game!"+
                            "\n       ........................................") ;
                    paneContainsAll.setBottom(text);
                    if ( val <= 0 ){
                        clock.stopTime();
                        showDialog();
                        Main.window.show();
                        gameStage.close();
                    }
                    l.setTextFill(Color.RED);
                    System.out.println(val);

                }
            }
            if ( p > positionNow){
                l.setTextFill(BLACK);
            }
            }
```
* Problem:
  - Too long if else statement hard to read and understand.
* Refactor
  - Extract part of that code to one method.
  ```java
   public void checkPosition(boolean correct, int p, Label l) {
        if (p < positionNow){
            l.setTextFill(GREEN);
        }
        if ( p == positionNow){
            if (correct){
                l.setTextFill(BLUE);
            }
            else{
                val -= 5 ;
                text.setText(textFormat(val));
                paneContainsAll.setBottom(text);
                if ( val <= 0 ){
                    clock.stopTime();
                    showDialog();
                    Main.window.show();
                    gameStage.close();
                }
                l.setTextFill(RED);
                System.out.println(val);
            }
        }
        if ( p > positionNow){
            l.setTextFill(BLACK);
        }
    }
    
       public void drawWord(boolean correct){
        if (time == 0){
            val -= 20;
        }
        int p = 0;
        String word = currentString;
        wordsForTyping.getChildren().clear();
        for (String s : word.split("")){
            Label l = new Label( s );
            checkPosition(correct, p, l);
            l.setPrefWidth(20);
            l.setAlignment( Pos.CENTER );
            l.setFont( new Font("Arial",50));
            wordsForTyping.getChildren().addAll(l);
            p++;
        }
    }
  ```
 * Benefits:
    - Making code more readable
    - Easier to detect bug in algorithm
    
 In the `src/application/windows/PlayGame.java`

https://github.com/Ing140943/TypingGame/blob/master/src/application/windows/PlayGame.java

consider this code: 
### showDialog method
```java
public void showDialog(){
        Alert alert = new Alert(Alert.AlertType.INFORMATION);
        if (countWords < 100){
            alert.setTitle("Game Over");
            alert.setHeaderText("You loose because your helth is Over! \n You have to practice more!");

        }
        else{
            alert.setTitle("Congratulations!!");
            alert.setHeaderText("You won this game!!!");

        }
        clock.stopTime();
        alert.showAndWait();
    }
```

* Problems:
  - alert variable should declare in constructor because another methods should able to call it too.(Pull up Field)
  - has duplicate code to call when call setTitle and setHeaderText
 * Refactor:
   - move alert variable to constructor
   - make method setTitleAndHeader
 ```java
     public void showDialog(){
        String setTitle;
        String setHeaderText;
        if (countWords < 100){
            setTitleAndHeader("Game Over", "You loose because your helth is Over! \n You have to practice more!");
        }
        else{
            setTitleAndHeader("Congratulations!!", "You won this game!!!");
        }
        clock.stopTime();
        alert.showAndWait();
    }

    public void setTitleAndHeader(String setTitle, String setHeaderText) {
        alert.setTitle(setTitle);
        alert.setHeaderText(setHeaderText);
    }
 ```
 * Benefits:
      - Code get more clean
      - Code is more readable
      - Eliminates duplication of variable alert

