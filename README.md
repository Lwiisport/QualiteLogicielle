# QualiteLogicielle
Dépôt GitHub pour le cours de Qualité Logicielle IMT Mines Alès

## Exercice 1 (Repo 1): Discover the Code & Rough Size


| Class          | LOC | NOM | Short description of responsibility          |
| -------------- | --- | --- | -------------------------------------------- |
| Bank           | 14  | 413 | Informations sur la Banque                   |
| BankAccount    | 20  | 462 | Gère les compte en banque des utilisateurs   |
| Person         | 23  | 325 | Utilisateurs de la banque                    |
| BankAccountApp | 2   | 491 | Application pour accéder au compte en banque |

Je pense que la taille des classes est en accord avec leur fonction. Sauf pour la classe `Person` qui récupère clairement trop d'informations et est trop grande par rapport à sa responsabilité.

## Exercice 2 (Repo 1): Cyclomatic Complexity on a Key Method

BankAccount : 
WMC = 20
CC = 33

Méthode = `WithdrawMoney`.


1. La complexité cyclomatique pour la méthode `WithdrawMoney(double withdrawAmount)` est de `5`.

```
public boolean withdrawMoney(double withdrawAmount) {

	if (withdrawAmount >= 0 && balance >= withdrawAmount && withdrawAmount <             withdrawLimit && withdrawAmount + amountWithdrawn <= withdrawLimit) 
	{
		balance = balance - withdrawAmount;
		success = true;
		amountWithdrawn += withdrawAmount;
	} 
	else 
	{
		success = false;
	}
return success;
}
```

Il y a 1 point au premier `if`, 1 point pour chaque `&` dans le `if`. Finalement, 1 point pour le `else`. Ce qui nous fait 5 points (1 `if`, 3 `&`, 1`else`).

2. La méthode `WithdrawMoney` contient un `if` et un `else`. Nous pourrions par exemple utilisé un `switch` afin de refactorisé cette fonction. Nous pourrions aussi ajouter les lignes `balance = balance - withdrawAmount;` et `amountWithdrawn += withdrawAmount;` dans une fonction externe qui ferai elle même le transfert d'argent. Cela permettrait d'avoir une meilleure maintenabilité et une meilleure lisibilité. Nous pourrions nommé cette méthode `UpdateBalance(double Amount)`. 

3. Voici la méthode modifié :

```
public boolean withdrawMoney(double withdrawAmount) 
{
	if (withdrawAmount >= 0 && balance >= withdrawAmount && withdrawAmount <             withdrawLimit && withdrawAmount + amountWithdrawn <= withdrawLimit) {
		success = UpdateBalance(withdrawAmount)
	} 
	else 
	{
		success = false;
	}
	return success;
}

  

public boolean UpdateBalance(double withdrawAmount) 
{
	balance = balance - withdrawAmount;
	amountWithdrawn += withdrawAmount;
	return true;
}
```

Une fois que l'on réanalyse le document, la complexité n'a pas changé.

## Exercice 3 (Repo 1): CK Metrics Across Classes: Who Looks "Smelly"?


1. Nous pouvons analyser les fichiers `Bank`, `BankAccount`, `Person`, `BankAccountApp` :


| Class          | LOC | WMC | CBO | Notes                                                                          |
| -------------- | --- | --- | --- | ------------------------------------------------------------------------------ |
| Bank           | 413 | 14  | 4   | la classe `Bank` à plus de lien avec d'autre classe.                           |
| BankAccount    | 462 | 20  | 3   | `BankAccount` est une des plus grande classe avec le plus de fonction interne. |
| Person         | 325 | 23  | 3   | `Person` est la classe avec le plus de fonctions tout en étant la plus petite. |
| BankAccountApp | 491 | 2   | 3   | Malgré seulement 2 fonctions, `BankAccountApp` est la classe la plus grande.   |

La classe `Person` a la `Weighted methods per class` la plus grande.
La classe `Bank` a la `Coupling between object classes` la plus grande.Nous pouvons analyser les
