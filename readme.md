# Writing Expressions in Davinci Resolve
You can use simple expressions or more complex `lua` script expressions.

## Simple expressions

## LUA basics

To enable `lua` mode, add a colon `:` to the beginning of your code. Use semicolons `;` to end a line. (You can not actually add new lines as this will break the code).
If you are in `lua` mode you need to `return` something from you script.

### If
In `lua` you can use `if then` conditions. Make sure to use `end` after the last return.

**Simple if else**
```lua
:if a > 1 then return 1 else return a end
```

**if elseif else**
```lua
:if a > 1 then return 1 elseif a < 1 return 0 else 0.5 end
```

**multiple conditions**
```lua
:if (a <= 1 and a >= 0) then return a elseif (a > 1) return 1 else 0 end
```

### Variables
Define a variable using an `=` and used it via its name.
```lua
:a=1;b=2;c=b/a;
```

:bMax=0.13;bMin=0.07; bWidth=0.09 * Controls.Size;if (bWidth <= bMax and bWidth >= bMin ) then return bWidth elseif(bWidth < bMin) then return bMin else return bMax end

## Custom controls

### Dropdown menu
To create a dropdown menu use a `combo box` control. You can provide any label you want, but the value you get will be the index of the selected item, starting with `0`.

So if you dropdown has 4 items you get the following:
- top -> `0`
- right -> `1`
- bottom -> `2`
- left -> `3`


# Examples

## A dropdown that alignes the corner of a box

![Bubble and nodes](assets/bubble.png)

### Setup
We have a `sRectangle` named `BubbleBody` with rounded corners of size `x1` and `y1`.
On top we have an _instance_ of this `sRectangle` named `Instance_BubbdleCorner` without rounded corners, _corner radius_, _offset x & y_ and _width_ and _height_ are detached. The size of this of instance is `x1/2` and `y1/2`. This small box is used to lay on top of one of the rounded corners to make it squared.
We also use a `sBoolean` set to `Union` to combine both into one shape afterwards.

The entire construct of nodes is in a _group_ named `Bubble`. On this group we have a custom control named `Tip` using a `combo box` with the four values: `top left`, `top right`, `bottom right`, `bottom left`. This is used to position the corner of the box.

### Expressions

On the `Instance_BubbdleCorner` we use the following expressions:
- width is set to: `BubbleBody.Width/2`
- height is set to: `BubbleBody.Height/2`
- X offset is set to: `:if(Bubble.Tip == 1 or Bubble.Tip == 3) then return BubbleBody.Translate.X + Width/2 else return BubbleBody.Translate.X - Width/2 end`
- Y Offset is set to: `:if(Bubble.Tip == 0 or Bubble.Tip == 1) then return BubbleBody.Translate.Y + Height/2 else return BubbleBody.Translate.Y - Height/2 end`