---
title: Telefonica Code Challenge 1
date: 2023-03-07 16:55:53
categories:
	- telefonica-challenge
---

## Enunciado:

![](/images/challenge/1/Challenge1.png)

---

## Solución:

Sin mucho misterio, se comienza con un problema sencillo para comprobar que podemos hacer bien la operativa con la plataforma de Telefónica.

Se pide el valor mínimo que tenemos que sacar en los dados para ganar a alguien de quien se da su resultado, por lo que no hacemos más que sumar los dos números, e imprimir uno más. Obviamente, teniendo en cuenta que si el contrario había sacado dos seises, es imposible sacar 13 en un dado.


``` python
cases = int(input())
for i in range(cases):
	data = input()

	# We obtain the total score given both dies
	result = sum(map(int, data.split(":")))
	
	# One more should be the expected outcome
	response = result + 1
	# If we go over the limit of 'two dies', return '-' instead
	if response > 12:
		response = "-"

	print(f'Case #{i+1}: {response}')
```