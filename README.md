# Boss Fighting

import random
import sys

#the player has apples, wood as consumtables. They have to keep their hp and action above 0.

player = { 
    "name": "p1", 
    "items" : ["key"],
    "apples" : 2,
    "wood" : 0,
    "action" : 100,
    "hp" : 100
}

rooms = {
    "room1" : "home",
    "room2" : "forest",
    "room4" : "bossHome"
}

def gameOver():
    print("Game over!")
    print("-------------------------------")
    print("to be continued!")
    sys.exit()
    return

def youWin():
    print("You succussfully killed the boss!!")
    print("Congrats!")
    print("-------------------------------")
    print("to be continued!")
    sys.exit()
    return

# check whether the game is over

def check():
    if(player["action"]<=0 or player["hp"]<=0):
        print("action: " + str(player["action"]))
        print("hp: " + str(player["hp"]))
        gameOver()

# eat can help increase 10 action but will consumpt an apple

def eat():
    if(player["apples"]>0):
        player["apples"] -= 1
        player["action"] += 10
        if (player["action"] >100):
            player["action"] = 100
        print("apples: " + str(player["apples"]))
        print("action: " + str(player["action"]))  
    else:
        print("You have no apples")

# rest can help increase 10 hp but lose 5 action

def rest():
    player["hp"] += 10
    if (player["hp"] > 100):
        player["hp"] = 100
    player["action"] -= 5
    print("hp: " + str(player["hp"]))
    print("action: " + str(player["action"])) 

# lumber will gain 1-3 wood

def lumber():
    woodHarvest = rollDice (1,3)
    player["wood"] += woodHarvest
    player["action"] -=10
    print("wood: " + str(player["wood"]))
    print("action: " + str(player["action"]))

# Once the user has 12 or more than 12 wood they can make the weapon

def makeWeapon():
    if (player["wood"]>=12):
        player["action"] -=20
        player["items"].append("weapon")
        print("Now you get a great weapon in your hand!")
    else:
        print("You don't have enough wood")

def rollDice(mini, maxi):
    outcome = random.randint (mini, maxi)
    print ("You roll a " + str(outcome) + " out of " + str(maxi))
    return outcome
 
# Born place: home

def goHome():
    print("Now you are at home")
    print("You can rest but you will be hungary")
    pcmd = input("options:[rest, eat, go to forest, make weapon, fight boss] >")
    if (pcmd == "rest"): 
        rest()
        check()
        goHome()
    elif (pcmd == "eat"):
        eat()
        goHome()
    elif (pcmd == "go to forest"):
        gotoForest()
    elif (pcmd == "make weapon"):
        makeWeapon()
        check()
        goHome()
    elif (pcmd == "fight boss"):
        fightBoss()
    else:
        print("You can't do that")
        goHome()

# Go to forest for wood and apples.
# If unlucky, the user may encounter forest protector

def gotoForest():
    print("Now you are at the forest")
    print("You can pick apple or lumber")
    pcmd = input("options: [go home, pick apple, lumber] >")
    if (pcmd == "go home"):
        goHome()
    elif (pcmd == "pick apple"):
        harvest = rollDice (0,2)
        player["action"] -= 5
        player["apples"] += harvest
        check()
        if (player["apples"] > 5):
            player["apples"] = 5
        print ("apples: " + str(player["apples"]))
        print ("action: " + str(player["action"]))
        gotoForest()
    elif (pcmd == "lumber"):
        luck = rollDice (0,10)
        if (luck > 7):
            print("You meet a forest protector")
            harm = rollDice (0,luck)
            print("You lose " + str (harm) + " hp")
            player["hp"] -= harm
            player["action"] -= 5
            check()
            print("hp: " + str(player["hp"]))
            print("action: " + str(player["action"]))
            gotoForest()
        else:
            print("luckily, no one is here")
            lumber()
            check()
            gotoForest()
    else:
        print("You can't do that")
        gotoForest()

# Once ready (mostly with the weapon), the user can fight against the boss

def fightBoss():
    print("Now you are challenging the boss!")
    print("Let's see your strength!")
    Capability = player["hp"] + player["action"]
    if ("weapon" in player["items"]):
        Capability += 200
    fight = rollDice (0,100)
    totalHarm = Capability + fight
    print("You deal " + str (totalHarm) + " harm to the boss")
    print("The boss has 280 hp in total")
    if (totalHarm >= 280):
        youWin()
    else:
        player["hp"] -= 50
        print ("You did not kill the boss")
        check()
        print("hp: " + str(player["hp"]))
        goHome()

def introduction():
    print("Good to see you again")
    print("Welcome to the game")
    print("You live in a world that has a evil boss and you need to kill him!")
    print("You can make weapon from the wood in the forest")
    print("Oh, and there is delicious apples")
    print("But be carefull of the guarder there")
    print("Wish you good luck")

def main():
    introduction()
    goHome()

main()
