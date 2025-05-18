# Counting how many specific elements are in a list

For example we have a list of numbers, which can be repeated.
```c#
List<int> numbers = new List<int>();
```

And we want to count how many `2` we have in the list.

# Solutions

## For loop

Go for each number in that list and check if it is the number you are looking for, then increase the counter.

*Boring*

Example:

```csharp
List<int> numbers = new List<int>();
numbers.Add(1);
numbers.Add(2);
numbers.Add(2);
numbers.Add(3);
numbers.Add(4);

int count = 0;
foreach (int number in numbers) {
    if (number == 2) {
        count++;
    }
}
```

## Remove

First we get how many elements are in the list `5`, then we get that count, before we remove the numbers, then we remove the number we want to count, and then only the rest of the numbers remain `3`. 

`5 - 3 = 2` basically we see how much we have removed from the list, when we removed the number we needed.

Example:

```csharp
List<int> numbers = new List<int>();
numbers.Add(1);
numbers.Add(2);
numbers.Add(2);
numbers.Add(3);
numbers.Add(4);

// 1, 2, 2, 3, 4 | length = 5
int length_before_removal = numbers.Count;
numbers.RemoveAll(num => num == 2);
// 1, 3, 4 | length = 3
// 5 - 3 = 2
int count = length_before_removal - numbers.Count;
```

What if you don't know how many elements are in the list and you want to count them all?

Example:

```csharp
List<int> numbers = new List<int>();
numbers.Add(1);
numbers.Add(2);
numbers.Add(2);
numbers.Add(3);
numbers.Add(4);

// key: number, value: count of that number
Dictionary<int, int> counting = new ();

// Create a hashset to remove duplicates, since we want only the numbers and not duplicates.
foreach (int eachNumber in new HashSet<int>(numbers)) {
    int length_before_removal = numbers.Count;
    numbers.RemoveAll(num => num == eachNumber);
    counting.Add(eachNumber, length_before_removal - numbers.Count);
}
```

