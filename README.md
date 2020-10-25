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
  - We can grouped the code together to one method.
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
  
  Benefit: The code is more readable and less duplication code.
 
