# Guessing game code

> import itertools
> import random
 
> digits = "0123456789"
> length = 4
> combinations = list(itertools.permutations(digits, length))

> def next_best_guess(combinations, last_try, correct, diff_position, total_correct):
>     if correct == 4:
>         return ('Correct Guess: ', last_try, correct)
>     
>     to_remove = correct + diff_position
>     filtered = []
>     for combination in combinations:
>         first = last_try[0] in combination
>         second = last_try[1] in combination
>         third = last_try[2] in combination
>         fourth = last_try[3] in combination
>         lst = [first, second, third, fourth]
>         
>         first_corr = last_try[0] == combination[0]
>         second_corr = last_try[1] == combination[1]
>         third_corr = last_try[2] == combination[2]
>         fourth_corr = last_try[3] == combination[3]
>         lst_corr = [first_corr, second_corr, third_corr, fourth_corr]
>         
>         if sum(lst) == to_remove and sum(lst_corr) == correct:
>             filtered.append(combination)
>     total_correct += to_remove
>     if last_try == ('1', '2', '3', '4'):
>         next_guess = ('5', '6', '7', '8')  
>     elif last_try == ('5', '6', '7', '8') and total_correct < 3:
>         combos09 = []
>         for combo in filtered:
>             if '0' in combo and '9' in combo:
>                 combos09.append(combo)
>         next_guess = random.choice(combos09)
>     else:
>         next_guess = random.choice(filtered)
>     if len(filtered) == 1:
>         print(F"This will be the correct guess: {next_guess}")
>     else:
>         print(F"Guess this: {next_guess}")
>     
>     return next_guess, filtered, total_correct
> 
> def get_valid_input(message, lower_limit, upper_limit):
>     while True:
>         try:
>             value = int(input(message))
>             if lower_limit <= value <= upper_limit:
>                 return value
>             else:
>                 print(f"Invalid input. Please enter a value between {lower_limit} and {upper_limit}.")
>         except ValueError:
>             print("Invalid input. Please enter an integer.")
>             
> def get_valid_input2(message):
>     while True:
>         try:
>             value = tuple(input(message))
>             if int(value[0]) != int(value[1]) and int(value[0]) != int(value[2])\
>             and int(value[0]) != int(value[3]) and int(value[1]) != int(value[2])\
>             and int(value[1]) != int(value[3]) and int(value[2]) != int(value[3]) and len(value) == 4:
>                 return value
>             else:
>                 print(f"Invalid input. Please enter 4 unique numbers between 0 and 9.")
>         except ValueError:
>             print("Invalid input. Please enter only integers with no spaces.")
> 
> input_tuple = get_valid_input2("Enter a guess of 4 different digits: ")
> 
> num_correct = get_valid_input("Enter the number of digits in correct position: ", 0, 4)
> diff_position = get_valid_input("Enter the number of digits correct but not in the right position: ", 0, 4)
> 
> total_correct = 0
> 
> while True:
>     next_guess, result, total_correct = next_best_guess(combinations, input_tuple, num_correct, diff_position, total_correct)
>     
>     if next_guess == 'Correct Guess: ':
>         print(next_guess + "".join(result))
>         break
>     
>     combinations = result
>     input_tuple = next_guess
>     
>     num_correct = get_valid_input("Enter the number of digits in correct position: ", 0, 4)
>     diff_position = get_valid_input("Enter the number of digits correct but not in the right position: ", 0, 4)
