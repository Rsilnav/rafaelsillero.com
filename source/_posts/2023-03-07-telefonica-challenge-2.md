---
title: Telefonica Code Challenge 2
date: 2023-03-07 16:55:56
categories:
	- telefonica-challenge
---

## Enunciado:

![](/images/challenge/2/Challenge2.png)

---

## Solución:

``` python
cases = int(input())
for case in range(cases):
	(num_poke, rows, cols) = map(int, input().split())

	# Get all the pokemons
	pokemons = [input().strip() for p in range(num_poke)]

	# Be sure all the input is in just one single string without spaces nor linebreaks
	text = ""
	for l in range(rows):
		text += str(input().strip().replace(" ", ""))
	
	while True:
		has_found = False

		for pokemon in pokemons:

			# Remove pokemon if found in the text
			find_straight = text.find(pokemon)
			if find_straight != -1:
				text = text[:find_straight] + text[find_straight+len(pokemon):]
				has_found = True

			# Remove pokemon if found reversed in the text
			find_reverse = text.find(pokemon[::-1])
			if find_reverse != -1:
				text = text[:find_reverse] + text[find_reverse+len(pokemon):]
				has_found = True

		# If it hasn't found anything after checking all pokemons, we are done
		if not has_found:
			break

	print(f'Case #{case+1}: {text}')
```