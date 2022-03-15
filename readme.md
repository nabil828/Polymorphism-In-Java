 # Polymorphism
 ###### [Reading time - 10 minutes]

 In Java, Polymorphism allows the same function to mean different things in different contexts.
 Without even knowing it, you have been using polymorphism!

 Remember function overloading? If yes, this is the first historical reference for polymorphism in programing. Later, the term shifted to refer to an object determining at the execution time which *overloaded* implementation should be executed for a member function.

 # Example
Suppose we have the following `Shape` class:
 ```Java
 public class Driver {
     static class Shape{
         void draw(){
             System.out.println("Shape has been drawn!");
         }
     };

     public static void main(String[] args) {
         Shape aShape = new Shape();
         aShape.draw();
     }
 }
 ```
 Output:

 ```
 Shape has been drawn!
 ```
Now, say we want to define shapes like a circle or a square and draw them. It might be intuitive to you here to use inheritance.

Since both *circle* and *square* classes are basically shapes and eventually they will have their own draw methods, it make sense to inherit the shape class as the following:


```Java
public class Driver {
    static class Shape{
        void draw(){
            System.out.println("Shape has been drawn!");
        }
    };

    static class Circle extends Shape{
        void draw(){
            System.out.println("Circle has been drawn!");
        }
    };

    static class Square extends Shape{
        void draw(){
            System.out.println("Square has been drawn!");
        }
    };

    public static void main(String[] args) {
        Shape aShape = new Circle();
        aShape.draw();
        Shape anotherShape = new Square();
        anotherShape.draw();
    }
}
```

Output:

```
Circle has been drawn!
Square has been drawn!
```

Notice, here we have used the parent class `Shape` to create both *circle* and *square* objects and yet `draw()` method was *binded* at the runtime not at the compilation time to the `Circle` and `Square` classes, respectively. This is polymorphism in action - one method in parent class got overridden in child classes.

---
Two rules of thumb here.

First,
> Java uses an object's dynamic type, not its name, to see which method to invoke.

Second,
> The object, not the variable, determines which definition of a method will be used.

---

Now, you might be asking yourself. Why we have created `aShape` with `Shape` class if we meant to assign it to `Circle` class eventually. To answer this, let us first check the following example:

```Java
public class Driver {
    static class Shape{
        void draw(){
            System.out.println("Shape has been drawn!");
        }
    };

    static class Circle extends Shape{
        void draw(){
            System.out.println("Circle has been drawn!");
        }
    };

    static class Square extends Shape{
        void draw(){
            System.out.println("Square has been drawn!");
        }
    };

    public static void main(String[] args) {
        Shape []shapeArray = new Shape[2];
        shapeArray[0] = new Circle();
        shapeArray[1] = new Square();

        for (Shape x : shapeArray){
            x.draw();
        }
    }
}

```
Notice here how late binding allowed us to call the `draw()` method on any object of type `Shape`. This is useful for any application that would require us to implement the different implementations for the same method name, again, depending on the context!

Before closing here, let me just remind you  that you might unwittingly used polymorphism in java. For example, most likely you have been calling  `toString()` method on any object in Java to get the *String* value of an object. Now this method is a member of the parent class `Object` and hence, it is inherited by all the other classes in Java, including your ones that did not exist until you have wrote them! This means that you can override this method to print the proper *String* representation of your object. May be like this:


```Java
public class Driver {
    static class Shape{
        void draw(){
            System.out.println("Shape has been drawn!");
        }
    };

    static class Circle extends Shape{
        public String toString(){
            return ("Circle has been drawn!");
        }
    };

    static class Square extends Shape{
        public String toString(){
            return ("Square has been drawn!");
        }
    };

    public static void main(String[] args) {
        Shape []shapeArray = new Shape[2];
        shapeArray[0] = new Circle();
        shapeArray[1] = new Square();

        for (Shape x : shapeArray){
            System.out.println(x.toString());
        }
    }
}

```

# Test Your Understanding - 1
What is the output of the following driver code snippet:

```Java
Shape aShape = new Circle();
aShape.draw();
aShape = new Square();
aShape.draw();

```

[A] -
```
Circle has been drawn!
Square has been drawn!
```

[B] -
```
Shape has been drawn!
Square has been drawn!
```

[C] -
```
Circle has been drawn!
Circle has been drawn!
```

[D] -
```
Shape has been drawn!
Shape has been drawn!
```





<details><summary>Answer</summary>

Option [A].

Remember:
> Java uses an object's dynamic type, not its name, to see which method to invoke.


</details>

# Test Your Understanding - 2

Is the following legal:

```Java
public class Driver {
    static class Shape{
    };

    static class Circle extends Shape{
        void draw(){
            System.out.println("Circle has been drawn!");
        }
    };

    static class Square extends Shape{
        void draw(){
            System.out.println("Square has been drawn!");
        }
    };


    public static void main(String[] args) {
        Circle aCircle = new Circle();
        Shape aShape = aCircle;
        aShape.draw();
    }
}
```


<details><summary>Answer</summary>

No. Java compiler will complain giving the following error:
```
Cannot resolve method 'draw' in 'Shape'
```

It is because `Shape` class does not have `draw()` member function.

Remember:
> Java uses an object's dynamic type, not its name, to see which method to invoke.

</details>
