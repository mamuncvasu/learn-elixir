# Elixir All [ Basics and Phoenix ]

## Ecto Getting Started Practice all

    |> Ei foldere e "friends"

##  Phoenix Project "pento" 

    |> Ei folder e "pento" 
    |> book: Tate B. Programming Phoenix LiveView. Interactive Elixir Web Programming 2023


## Learn enough elixir for Phoenix [Source : https://elixirschool.com/]

  ### iex and helper [h/i/t/r]

    Frequently Use: |> iex |> h String.upcase [use tab for autocomplete] |> i String.downcase

	terminal |> iex > IO.puts("Hello, " <> "world.")
	autocomplete |> <tab> key
	terminal > iex -S mix  # run iex in Application Context (rails c er moto)

  #### h helper 

	iex > h i 			# i helper ki kaj kore tar documentation
	iex > h (is_binary)
	iex > h(String)  	# Documentation of String Module
	iex > h(String.slice)  # Documentation of String Module er "slice" method

  #### i helper for Basic Details of Module/function/...

	iex > i h   # h helper ki kore tar documentation with example
	iex > i Map
	iex > i String

  #### t helper tells us about Types available
	
    iex > t String

  #### r helper for recompile a Module
	
    iex > r MyProject


## Pattren Matching (=) and pin operator(^)
    elixir এ = মানে শুধু assign না , ২পাশের ডাটা এবং ডাটা টাইপ match করে।  যেমন : x =2  ও 2=x প্যাটার্ন match করে কিন্তু 1=x তে error খায় , আবার x = 1 দিলে আগের ডাটার জায়গায় rebind হয়। যদি আগের ডাটা replace না করে pattern matching করতে চাই তবে pin operator ব্যবহার করতে হয়।  যেমনঃ x = 1 দিলে আগের value কে রিপ্লেস করে , কিন্তু ^x = 1 হলে আগের data র সাথে match হচ্ছে কিন শুধু সেটা দেখে

## Pipe Operator [ 1st fun |> 2nd fun |> .... rightmost will execute last] 

    The pipe takes the result on the left, and passes it to the right hand side.
        other_function() |> new_function() |> baz() |> bar() |> foo()
        Ex : a = "Elixir rocks" |> String.upcase() |> String.split()


## collections [List, Tuple, Maps]

  ### List (3rd bracket er moddhe)
        1)Lists are collections of values which may include multiple types; lists may also include non-unique values.
            list1 = [3.14, 42, :pie, "Apple"]  list2 = [42, "bar"]
        2)List addition and subtraction
            list = list1 ++ list2
            list = list1 -- list2
        3)List er Head(1st element) and Tail(all without 1st element)
            hd [3.14, :pie, "Apple"]
            tl [3.14, :pie, "Apple"]
        4)List with pattern matching
            [head | tail] = [3.14, :pie, "Apple"]
    
  ### Tuples(2nd bracket er moddhe)

    Tuple list এর চেয়ে faster,কিন্তু সেভ করতে সময় মেমোরিতে পাশাপাশি সেভ করতে হয়।  লিস্ট কে কিন্তু অ্যাড / ডিলেশন করা faster 
    tuple1 = {3.14, :pie, "Apple"}

  ### Keyword Lists

    keyword list is a special-case list |> জোড়ায় জোড়ায় tuples দিয়ে তৈরী একটা list,যে tuples এর key হবে একটা atom.
    এর ৩ টি বৈশিষ্ট্য : ১) key হবে একটা atom  ২) key-value যেভাবে দেয়া হবে সেভাবেই সেভ হবে ,sorting/ordering হবে না  ৩) key কে unique হতে হবে না। 
    a = [{:foo, "bar"}, {:hello, "world"}]

  ### Maps [%{}] --- Must for phoenix

    %{} এর মধ্যে koma separated  key-value pairs, যেখানে key হবে atoms /variables 

        map = %{:foo => "bar", "hello" => :world}
        
    map এ কোনো key duplicate হলে last এরটা আগেরটাকে রিপ্লেস করে।  একটা map থেকে ডাটা পাওয়ার জন্য  map.key , নতুন ডাটা insert এর জন্য Map.put (map এর নাম, key-value ):

        map = %{hello: "world"}
        map.hello   # get value of key "hello"
        %{map | hello: "baz"} # update existing value
        Map.put(map, :foo, "baz")  # add new item to map
    
    
## Enumerations [গণনা করা]--[enum,stream]--- must for Phoenix

        Elixir  এ  Collection-কে গণনা (enumerate ) করার eager way হচ্ছে enum  [ আর Lazy way হচ্ছে stream ]. enum/stream  উভয়েই হচ্ছে module যাদের স্পেশাল function আছে enumerable object কে [List , Map , Range ] নিয়ে কাজ করার।  যেমনঃ  sorting , sum , count , fetch , find ..... etc .

        1) Enum.all?(["foo", "bar", "hello"], fn(s) -> String.length(s) == 3 end)  ## collection এর সকল element এর জন্য condition সঠিক হলে true , নাহলে false 

        2) Enum.any?(["foo", "bar", "hello"], fn(s) -> String.length(s) == 5 end)  ## collection এর যেকোনো একটা  element এর জন্য condition সঠিক হলে true

        3) Enum.chunk_every([1, 2, 3, 4, 5, 6], 2)  ## collection কে ২টা (nth ) element করে sub -group করবে 

        4) Enum.each(["one", "two", "three"], fn(s) -> IO.puts(s) end)  ## প্রতিটা element কে ১বাই ১ traverse করবে, কিন্তু collection এর কোনো data manipulate করবে না 

        5) Enum.map([0, 1, 2, 3], fn(x) -> x - 1 end)  ## Enum .map/২  প্রতিটা element কে ১বাই ১ traverse করে  collection এর  data manipulate করবে।  

        6) Enum.min([5, 3, 0, -1]) Enum.max([5, 3, 0, -1]  ## minimum or maximum value of this collection

        7) Enum.map([1,2,3], &(&1 + 3))     ## Enum using the Capture operator (&) 


## Captured anonymous function [ fn a,b -> a+b end ~ &(&1 + &2)]  --- Must for Phoenix

        elixir এর annonymous function লিখার way , [ add = fn a,b -> a+b end ] এটা ১টা normal function এর শর্টকাট হলো [ add = &(&1+&2) ] , এখানে ১ম & মানে annonymous function এর declaration , &1 মানে ১ম parameter  , &2 মানে ২য় parameter , () এর মধ্যে method এর body

            add = fn a,b -> a+b end
            or, add = &(&1 + &2)   ## annonymous function as a variable
            result = add.(3, 5)  ## function call 
            IO.inspect(result) 
   
## DateTime [only date/time, with/without timezone]

    t = Time.utc_now  # only hour,minute, second ~U[2025-07-15 10:49:32.988792Z]
    t.hour
    
    {:ok, date} = Date.new(2020, 12, 12) # shudhu Date dibe, ok hole ta "date" variable e thakbe
    {:error, date} = Date.new(2020, 13, 12)  # error khabe tai "date" e ":invalid_date" save hobe
    
    NaiveDateTime.utc_now   # Date and time without tomezone
    DateTime.from_naive(~N[2016-05-24 13:26:08.003], "Etc/UTC")   # Date and time with tomezone
    or, DateTime.from_naive(NaiveDateTime.utc_now, "Etc/UTC")
    
## Sigils  [~c/~r/~s/~w/~N/~U/~T] EX: re = ~r|elixir|

    sigils হচ্ছে অক্ষর/বর্ণ নিয়ে কাজ করার একটা বিকল্প পদ্ধতি।  যেমন : string নিয়ে কাজ করার জন্য  elixir এ নির্দিষ্ট কিছু module আছে , একই কাজ sigils দিয়েও করা যায়। 
    sigil ১টা tilde ~ এবং ১জোড়া delimiter দিয়ে লেখা হয়।Delimiter হতে পারে <>,(),{},[],।।, //, '',"" এদের যেকোনো ১ জোড়া।  যেমঃ ~s/the cat man/  স্ট্রিং প্রকাশ করে। 
        
        # sigils diye regular expression
        re = ~r/elixir/  
        "elixir" =~ re  # true hobe
        
        ~w/i love elixir school/  # wordlist
        DateTime.from_iso8601("2015-01-23 23:50:07Z") == {:ok, ~U[2015-01-23 23:50:07Z], 0}  # Sigil's DateTime and regular Dattime will be same.
        ~T[19:39:31.056226]   # is equvalent to Time.utc_now
    
## Create custom Sigils  [create ~p sigil and use it]

    defmodule MySigils do
      def sigil_p(string, []), do: String.upcase(string)
      def sigil_REV(string, []), do: String.reverse(string)
    end

    #use ~p and ~REV sigil
    import MySigils
    ~p/elixir school/
    ~REV<foobar>
    
## Strings 

    string = <<104,101,108,108,111>>   # "hello" print korbe byte theke string
    ?Z      # Z er unicode dibe

    string = "\u0061\u0301"         # String declaration
    String.codepoints string        # charlist e dekhabe, to know more > h String.codepoints
    String.graphemes string
    String.length "Hello"
    String.replace("Hello", "e", "a")
    String.duplicate("Oh my ", 3)
    String.split("Hello World", " ")
    String.to_charlist "Hello World"  # charlist e convert korbe           


## Control Structure [if/case/cond/with]

  ### if-else

    One-liner if-else |> if(foo, do: bar)  || if(foo, do: bar, else: baz)
    Block Style if-else |>
    if foo do
        bar
    else
        baz
    end

  ### cond [match conditions rather than values]

    অনেকগুলো condition এর মধ্যে যেটা true  হবে  সেটার মেসেজ দেখাবে 
        cond do
        a + b == 5 ->
            "This will not be true"
        a * b == 3 ->
            "Nor this"
        a + b == 2 ->
            "But this will"
        end

  ### case [for pattern matching]

    case হচ্ছে elixir এর switch , যে case মিলবে সেই block রান করবে। কোনো case না মিললে _ ব্লক রান করবে। যেমনঃ variable (month ) এর মানকে case দিয়ে চেক করা হলে কোনো case না মিলাতে _ ব্লক রান করে। 

        month = :june
        case month do
            :january -> IO.puts "this is 1st month"
            :february -> IO.puts "this is 2nd month"
            _ -> 
                IO.puts "month between 3-12"
                IO.puts "Don't Panic "
        end

  ### with [nested case implement korte use hoy]

        with এর পর কমা separated condition সবগুলো true হলেই do ব্লক এর কোড রান করবে। যেমনঃ নিচের function টিতে ২টা optional parameter আছে , কেবল ২টা parameter (width, height ) থাকলেই কেবল with এর ব্লক কাজ করবে আর area return করবে। 

            def area(opts) do
                with {:ok, width} <- Map.fetch(opts, :width), {:ok, height} <- Map.fetch(opts, :height) do
                    {:ok, width * height}
                end
            end
            
            # Example 02: ২টা condition true না হলে else-error ব্লক কাজ করবে 
            user2 = %{first: "Sean"}
            with {:ok, first} <- Map.fetch(user2, :first),{:ok, last} <- Map.fetch(user2, :last) do
                last <> ", " <> first
                IO.puts "with block 2 run"
            else
                :error -> IO.puts "with block 2 failed"
            end
    

## Function [named,private, polimorphisom] 

    function এর full format:  [def hello do "hello" end]  এবং short format : [ def hello, do: "hello"] এতে end থাকবে না কিন্তু ১টা , ১টা : থাকবে। private function এর format : [defp hello do "hello" end  অথবা defp hello, do: "hello"] ১টা এক্সট্রা p থাকে। একই নামে একাধিক function থাকতে পারে তবে parameter এর নাম্বার আলাদা হতে হবে, এটাকে elixir এর polimorphism বলা যায়।    

  ### Example 01 : shortform: 1 public & 1 private method 
    defmodule Greeter do
      def hello(name), do: phrase() <> name
      def hello(name,age), do: phrase() <> name <> age
      defp phrase, do: "Hello, "
    end
    Greeter.hello("mamun","12")
    Greeter.hello("mamun")

  ### Example 02: FUllform: 1 public(1/2 params) & 1 private method
    defmodule Greeter do
    	def hello(name) do
    		phrase() <> name
    	end
        def hello(name,age) do 
        	phrase() <> name <> age
        end
        defp phrase do
        	"Hello, "
     	end
    end
    Greeter.hello("mamun","12")
    Greeter.hello("mamun")


## Function with default parameter value

    ১টা function এর কোনো parameter এর default parameter সেট করতে হলে \\ দিয়ে value দিতে হয়। যেমনঃ নিচের function এর ২টা parameter (name ,language_code) এর মধ্যে language_code না দিলে default value হবে "en " 

        defmodule Greeter do
        def hello(name, language_code \\ "en") do
            phrase(language_code) <> name
        end
        defp phrase("en"), do: "Hello, "
        defp phrase("es"), do: "Hola, "
        end


## Annonymous function
    এটা ১টা normal function  [ add = fn a,b -> a+b end ],  এর শর্টকাট হলো [ add = &(&1+&2) ] , এখানে ১ম & মানে annonymous function এর declaration , &1 মানে ১ম parameter  , &2 মানে ২য় parameter , () এর মধ্যে method এর body

        sum = &(&1 + &2)
        sum.(2, 3)

## Function with guards [conditional function]

    function এর declaration এর সাথেই একটা condition থাকবে। condition true হলেই কেবল functionটা রান করবে, না হলে function রান করবে না। 

        def hello(name) when is_binary(name) do
            phrase() <> name
        end
    
## Function for pattern matching

    variable কে ১টা function হিসেবে ব্যবহার করা যায়, এটাকে pattern matching এর জন্য ব্যবহার করা হয়। 

        handle_result = fn
            {:ok, result} -> IO.puts "Handling result..."
            {:ok, _} -> IO.puts "This would be never run as previous will be matched beforehand."
            {:error} -> IO.puts "An error has occurred!"
        end
        handle_result.({:ok, 12})  # variable function ke call kora
    
## Modules [Organise/Grouping function]

    module এর single name দিয়ে হয় , তবে nested module লিখতে হলে ডট ( .) দিয়ে separate করে লিখতে হয়(Example.Greetings)। module এর নাম capital letter দিয়ে  শুরু হয়। 

        defmodule Example do
        def greeting(name) do
            "Hello #{name}."
        end
        end
        Example.greeting "Sean"

## Nested Modules
 
    defmodule Example.Greetings do
      def morning(name) do
        "Good morning #{name}."
      end
    end
    Example.Greetings.morning "Sean"
    
## Module Attributes [module wise instance variable]

    module এর মধ্যে module wise usable ভ্যারিয়েবল @ দিয়ে লেখা হয় , অনেকটা ruby এর instance variable এর মতো। 

    defmodule Example do
      @greeting "Hello"
    
      def greeting(name) do
        ~s(#{@greeting} #{name}.)
      end
    end
    
## Struct with Module [structure diye constractor]

    module এর মধ্যে struct রাখা হলে ওকে দিয়ে module কে কল করতে সময় default  data সেট করা যায়। struct ছাড়া call করতে হলে [ modulename .functionname ] এই format এ call দিতে হয় , struct সহ call করতে সময় % দিয়ে শুরু করতে হয়। 

    defmodule Example.User do
      defstruct name: "Sean", roles: []
    end
    %Example.User{name: "Steve", roles: [:manager]}
    
## @derive with struct  [for inspect]

    defmodule Example.User do
        @derive {Inspect, only: [:name]}
            defstruct name: nil, roles: []
        end
    sean = %Example.User{name: "Sean"}
    inspect(sean)


## Module aliasing

    aliasing করার পর শুধু  function এর নাম দিয়ে call করা যাবে, যদি অন্য নামে alias করা হয় তবে সেই নাম দিয়ে call করতে হয়।  

    defmodule Sayings.Greetings do
      def basic(name), do: "Hi, #{name}"
    end

    defmodule Example do
      alias Sayings.Greetings
      def greeting(name), do: Greetings.basic(name)     # aliasing করার পর শুধু  function এর নাম দিয়ে call করা
    end

    defmodule Example do
      alias Sayings.Greetings, as: Hi                   # 1st module কে  "Hi" নামে call করা যাবে
      def print_message(name), do: Hi.basic(name)
    end

## Import Module and Filter [Both functions & Macros]

    By default সব  functions এবং  macros কে  import করে , শুধু function গুলোকে import করতে চাইলে [import List, only: :functions], শুধু macros গুলোকে import করতে চাইলে [import List, only: :macros], শুধু specific ১টা function import করতে চাইলে (import List, only: [last: 1] ), কিন্তু ১টা function ছাড়া বাকি সবগুলো function কে import করতে (import List, except: [last: 1]) এভাবে লিখতে হয়। 

        import List
        import List, only: [last: 1]
        import List, except: [last: 1]
        import List, only: :functions
        import List, only: :macros


## Require Module [Only Macros]
    
    require কে ব্যবহার করা মানে ওই module এর সব macros কে ব্যবহার করা যাবে। 

    defmodule Example do
      require SuperMacros
      SuperMacros.do_stuff
    end
    

## Module Use Macro [বুঝিনি]

    defmodule Hello do
      defmacro __using__(_opts) do
        quote do
          def hello(name), do: "Hi, #{name}"
        end
      end
    end
    
    defmodule Example do
      use Hello
    end