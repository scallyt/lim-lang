# Lim Language

## Syntax

### Identifiers

Identifiers are case-sensitive and can contain letters, digits, and underscores.

## Declaring Variables
 Variables are basicly constants if you declare them with $ sign its weel be a reactive variable.
```rs
// U can use let keyword but is not necesary you only need in in the any or auto types
// like:
int alma = 55;
// or
// Thats the same with int, float, string, array
let int alma = 55;

// but with the auto or any type its gonno give a compiler error
any alma = "String"; // Error
auto alma = "String"; // Error
// but
let any alma = "String"; // Works fine
let auto alma = "String"; // Works well

// Declarring lists
array<int> alma = [1,2,3,4,5,6];
let array<int> alma = [2,3,4,5,6,7,8,9];
// or you can have multipla type but their index is always the same
array<int, string> alma = [1, "alma"]; // Its work fine
array<int, string> alma = [1, 2]; // This give a compiler error
array<int, string> alma = [1, 2, "alma"]; // This give a compiler error
array<int, int, string> alma = [1, 2, "alma"]; // Its work fine
//but you can use a sorther version The indexing count by 0. 
array<int, string[2]> alma = [1, 2, "alma"];

// Max Value
// You can give a max value to a type like:
let string<12> $alma = "Asd"; // This is not an Array its  int this code the 12 is meaning the charachter
let int $alma<3> = 3; // And in this code is meaning the max value

// In array you can use it with the multipla type like:
let array<int<3>, string[2]<4>> = [1]; // you can spacify this with between the <> charachters
```

## Declaring Functions

function declaration
if you want to declar a function you have 2 option:
- reactive the reactive need to have **return** keyword
- normal the normal dont need to have **return** keyword but you need to define the return type to **$nr** but you can have return type AND returnable value if a value is null its gonna give a compiler error

normal functions dont have return state but the reactive need to return a value

func is can be async but func is can by only sequential
```rs
// function declaration
// In this code you need to specify the return type to $nr the
fun alma() -> $nr {
    io::print("alma");
}

// reactive function
fun $alma() -> int {
    io::print("alma");

    return 10;
}

fun asyncTask() {
    
}

func sequentialTask() {

}
```

```rs
// If its reactive
$alma();
// or if its not reactive
alma()
```

## If else

```rs
    if conditon {

    } else {

    } if else condition {

    }
```

## Switch
in this language we call switch to  like:
```rs
match $varible {
    $10 -> print("Hellow 10")
    $20 -> print("Hellow 10")
    $30 -> print("Hellow 10")
}
```

## Try catch

```rs
// the boomer way to error handeling (and the simplest way)
try {

} catch (ErrorType e) {
    io::println(e.message)
}
```

#Memory menegment
```rs
// Example:
let int $x = 3;
// Using ref keyword for references
ref<int> r = x;  // 'r' is a reference to 'x'

// Using ptr keyword for pointers
ptr<int> p = &x; // 'p' is a pointer to 'x'

// Dereferencing and modifying
io.println(*p);  // Prints the value of 'x'
*p = 20;         // Changes 'x' to 20

// Functions with pointers
func changeValue(ptr<int> val) {
    *val = 100;  // Modifies the original variable
}
```

## Import system
```rs
// Module system:
// You only import the module its work fine.
// private, protected, public

// main.lim
import "random"; { /*if you want you can specify what class or what class & function */ random::$randomint }
import "io";

module main {
    fun main() -> $nr {
        io::println("Hello world");
        random::$randomint();
    }
}


// random.lim
import "math";

module random {
    fun $randomint() -> int {
        let int random_number = math::random_int(1,100);
        return random_number;
    }
}

// public, protected, private

module mymodule {
    // This function is public, so it can be accessed by other modules
    public fun hello() -> $nr {
        io::print("Hello, world!");
    }

    // Private function, only accessible within 'mymodule'
    private fun internalFunction() -> $nr {
        io::print("This is internal.");
    }

    // Protected function, accessible by derived modules
    protected fun protectedFunction() -> $nr {
        io::print("Protected access");
    }
}

```

## OOP
```rs
module main {
    class Person {
        public let string name;
        protected let int age;

        public fun greet() -> $nr {
            io::print("Hello, my name is " + name);
        }
    }

    class Employee : Person {
        private let int employeeId;

        public fun showId() -> $nr {
            io::print("My ID is: " + employeeId);
        }
    }

    fun main() -> $nr {
        let Person john = new Person("John", 30);
        john.greet();

        let Employee alice = new Employee("Alice", 25, 12345);
        alice.greet();
        alice.showId();
    }
}

```



# Exlamples
### Simple backend API:
1.
```rs
import "http";
import "json";

module main {
    struct User {
        let string name;
        let int age;
    }

    fun alma();
    fun almaPost();

    fun main() ->$nr {
        http::GET("/") -> fun HelloWorld(http::request, http::response) -> http::response {
            return http::response(json::encode("Hello wolrd"));
        }

        http::GET("/alma") -> alma();
        http::POST("/post") -> almaPost();
        io::println("Server listening on port 8080!");
        http:StartServer(8080);
    }

    fun alma(http::request,http::response) -> http::response {
        return http::response(json::encode("Alma"));
    }

    fun almaPost(http::request req, http::response res) -> http::response {
        let string requestBody = http::request.body;

        let User user = json::decode(requestBody);

        let string response = json::encode("Received name: " + user.name + ", age: " + user.age.toString());

        return http::res(response);
    }
  }
```