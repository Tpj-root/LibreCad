# LibreCad

https://librecad.org/


Dependencies list

```
sudo apt install xdotool
https://github.com/LibreCAD/LibreCAD/releases/download/2.2.1.1_rc-latest/LibreCAD-v2.2.1.1-9-g5ad9b999-x86_64.AppImage
```



I didn't have time to create a plugin, so I made a script for automation.





Automatic script for drawing in the LibreCAD library via a Bash script.

# Hello World with xdotool

```
function libcad_test() {
    sleep 5
    xdotool type "Hello World"
    #xdotool key Return  # Press Enter after typing
}
```




### Commands

https://wiki.librecad.org/index.php/Commands
https://wiki.librecad.org/index.php?title=A_short_manual_for_use_from_the_command_line
https://wiki.librecad.org/index.php?title=LibreCAD_users_Manual#Using_Command_Line
https://wiki.librecad.org/index.php?title=Math_bits



- `100,100` → **Absolute coordinate**  
  Draws to the point (100,100) from origin.

- `@0,-20` → **Relative coordinate**  
  Moves 0 units in X and -20 units in Y **from the last point**.


In LibreCAD:

- Use `x,y` for fixed points.  
- Use `@x,y` to draw relative to the last point.




```
function libcad_square() {
    delay=0.5  # Set your sleep delay here
    sleep 5

    xdotool type "line"
    xdotool key Return
    sleep $delay

    xdotool type "0,0"
    xdotool key Return
    sleep $delay

    xdotool type "0,100"
    xdotool key Return
    sleep $delay

    xdotool type "100,100"
    xdotool key Return
    sleep $delay

    xdotool type "@0,-20"
    xdotool key Return
    sleep $delay

    xdotool type "100,0"
    xdotool key Return
    sleep $delay

    xdotool type "0,0"
    xdotool key Return
    sleep $delay

    xdotool type "k"
    xdotool key Return
}

```


Loop Method DrawLine



```
function libcad_square2() {
    delay=1
    sleep 5

    while read -r line; do
        xdotool type "$line"
        xdotool key Return
        sleep $delay
    done <<EOF
line
0,0
0,100
100,100
@0,-20
100,0
0,0
k
EOF
}
```


### DrawPoint 

```
function libcad_square2() {
    delay=1
    sleep 5

    while read -r line; do
        xdotool type "$line"
        xdotool key Return
        sleep $delay
    done <<EOF
point
0,0
0,100
100,100
@0,-20
100,0
0,0
k
EOF
}
```





Paste multiple commands

```
line
;0,0
;0,100
;100,100
;@0,-20
;100,0
;0,0
;k



```

### star

```
line
50,0
60,35
95,35
65,55
75,90
50,70
25,90
35,55
5,35
40,35
50,0
k



```



```

function libcad_spiral_square() {
    delay=0.5
    sleep 5

    while read -r line; do
        xdotool type "$line"
        xdotool key Return
        sleep $delay
    done <<EOF
line
0,0
100,0
100,90
10,90
10,10
90,10
90,80
20,80
20,20
80,20
80,70
30,70
30,30
70,30
70,60
40,60
40,40
60,40
60,50
50,50
k
EOF
}


```




```
function libcad_spiral_square_loop() {
    delay=0.5
    sleep 5

    xdotool type "line"
    xdotool key Return
    sleep $delay

    # Start coordinates
    x=0
    y=0
    step=100

    dir=0  # 0=right, 1=up, 2=left, 3=down

    for i in {1..10}; do
        case $dir in
            0) ((x+=step)) ;;  # right
            1) ((y+=step)) ;;  # up
            2) ((x-=step)) ;;  # left
            3) ((y-=step)) ;;  # down
        esac

        xdotool type "${x},${y}"
        xdotool key Return
        sleep $delay

        dir=$(( (dir + 1) % 4 ))  # cycle direction
        ((step-=10))  # decrease step each loop
    done

    xdotool type "k"
    xdotool key Return
}



```










TOdo:
https://www.youtube.com/watch?v=4isa_GsPL4Y&ab_channel=RcCarFan01
https://wiki.librecad.org/index.php/LibreCAD_3_-_Plugin_creation
https://github.com/LibreCAD/LibreCAD/tree/master/plugins/sample


