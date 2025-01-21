# SQL-Island
SQL-Island
SQL Island is a fun introduction to learning and using SQL. The challenge can solved on this website SQL ISLAND
This is my approach to solving the SQL Island challenge.

Question and Answer
**QUESTION:** It seems there are a few people living in these villages. How can I see a list of all inhabitants?

SELECT *
FROM INHABITANT;

**QUESTION:** Man! I'm hungry. I will go and find a butcher to ask for some free sausages.

SELECT *
FROM INHABITANT
WHERE job = 'butcher';

**QUESTION:** Thank you, Edward! Okay, let's see who is friendly on this island...

SELECT *
FROM INHABITANT
WHERE state = 'friendly';

**QUESTION**: There is no way around getting a sword for myself. I will now try to find a friendly weaponsmith to forge me one.

SELECT *
FROM INHABITANT
WHERE state = 'friendly' AND job = 'weaponsmith';

**QUESTION:** Oh, that does not look good. Maybe other friendly smiths can help you out, e.g. a blacksmith. Try out: job LIKE '%smith' to find all inhabitants whose job ends with 'smith'.

SELECT *
FROM INHABITANT
WHERE state = 'friendly' AND job LIKE '%smith';

**QUESTION:** That looks better! I will go and visit those smiths. Hi stranger! Where are you going? I'm Paul, I'm the major of Monkeycity. I will go ahead and register you as a citizen.

INSERT INTO INHABITANT (name, villageid, gender, job, gold, state)
VALUES ('Stranger', 1, '?', '?', 0, '?');

**QUESTION:** No need to call me stranger! What's my personid?

SELECT personid
FROM INHABITANT
WHERE name = 'Stranger';

NOTE: This returns a personid of 20

**QUESTION:** Hi Ernest! How much is a sword? I can offer to make you a sword for 150 gold. That's the cheapest you will find! How much gold do you have?

SELECT gold
FROM INHABITANT
WHERE personid = 20;

**QUESTION:** Damn! No mon, no fun. There has to be another option to earn gold other than going to work. Maybe I could collect ownerless items and sell them! Can I make a list of all items that don't belong to anyone?

SELECT *
FROM ITEM
WHERE owner ISNULL;

**QUESTION:** So much cool stuff! Yay, a coffee cup. Let's collect it!

UPDATE ITEM
SET owner = 20
WHERE item = 'coffee cup';

**QUESTION:** Do you know a trick how to collect all the ownerless items?

UPDATE ITEM
SET owner = 20
WHERE owner ISNULL;

**QUESTION:** Now list all of the items I have!

SELECT *
FROM ITEM
WHERE owner = 20;

**QUESTION:** Find a friendly inhabitant who is either a dealer or a merchant. Maybe they want to buy some of my items.

SELECT *
FROM INHABITANT
WHERE state = 'friendly'
AND job = 'dealer' OR job = 'merchant';

**QUESTION:** I'd like to get the ring and the teapot. The rest is nothing but scrap. Please give me the two items. My personid is 15.

UPDATE ITEM
SET owner = 15
WHERE item IN ('teapot', 'ring');

**QUESTION:** Thank you! Here, some gold!

UPDATE INHABITANT
SET gold = gold + 120
WHERE personid = 20;

**QUESTION:** Unfortunately, that's not enough gold to buy a sword. Seems like I do have to work after all. Maybe it's not a bad idea to change my name from Stranger to my real name before I will apply for a job.

UPDATE INHABITANT
SET name = 'Sriya'
WHERE name = 'Stranger';

**QUESTION:** Since baking is one of my hobbies, why not find a baker who I can work for?

SELECT *
FROM INHABITANT
WHERE job = 'baker'
ORDER BY gold DESC;

Aha, Paul! I know him! Hi, you again! So, Sriya is your name. I saw you want to work as a baker? Okay! You will be paid 1 gold for 100 bread rolls. (8 hours later...) Here, I made ten thousand bread rolls! I quit! This should be enough money to buy a sword. Let's see what happens with my gold balance. Here's your new sword, Ahmod Ayodolo! Now you can go everywhere. My name is Ahmed Ayodele! Thanks anyway!

**QUESTION:** Is there a pilot on this island by any chance? He could fly me home.

SELECT *
FROM INHABITANT
WHERE job = 'pilot';

personid	name	villageid	gender	job	gold	state
8	Arthur Tailor	2	m	pilot	490	kidnapped

Oh no, his state is 'kidnapped'. Horrible, the pilot is held captive by Dirty Dieter!

**QUESTION:** I will show you a trick how to find out the name of the village where Dirty Dieter lives.

SELECT village.name
FROM VILLAGE, INHABITANT
WHERE village.villageid = inhabitant.villageid
AND inhabitant.name = 'Dirty Dieter';

NOTE: This returns the village name as 'Onionville'.

**QUESTION:** Find out the chief's name of the village Onionville?

SELECT inhabitant.name
FROM VILLAGE, INHABITANT
ON village.villageid = inhabitant.villageid
WHERE village.name = 'Onionville'
AND village.chief = inhabitant.personid;

This returns the inhabitant name as Fred Dix

I've got it! I will visit Fred and ask him about Dirty Dieter and the pilot.

**QUESTION:** Um, how many inhabitants does Onionville have?

SELECT COUNT(*)
FROM INHABITANT, VILLAGE
WHERE village.villageid = inhabitant.villageid
AND village.name = 'Onionville';

This returns a total count of 8

**QUESTION:** Hello Sriya, the pilot is held captive by Dirty Dieter in his sister's house. Shall I tell you how many women there are in Onionville? Nah, you can figure it out by yourself!

SELECT COUNT(*)
FROM VILLAGE, INHABITANT
ON village.villageid = inhabitant.villageid
WHERE village.name = 'Onionville'
AND inhabitant.gender = 'f';

This returns a total count of 1

**QUESTION:** Oh, only one woman. What's her name?

SELECT inhabitant.name
FROM VILLAGE, INHABITANT
ON village.villageid = inhabitant.villageid
WHERE village.name = 'Onionville'
AND inhabitant.gender = 'f';

This returns the name: Dirty Diane

Let's go!

**QUESTION:** Sriya, if you hand me over the entire property of our nearby village Cucumbertown, I will release the pilot. I will show you now what this property consists of.

SELECT SUM(inhabitant.gold)
FROM INHABITANT, VILLAGE
WHERE village.villageid = inhabitant.villageid
AND village.name = 'Cucumbertown';

This return the sum of inhabitant gold to be 8980. QUESTION: Oh no, baking bread alone can't solve my problems. If I continue working and selling items though, I could earn more gold than the worth of gold inventories of all bakers, dealers and merchants together. How much gold is that?

SELECT SUM(gold)
FROM INHABITANT
WHERE job = 'baker'
OR job = 'dealer'
OR job = 'merchant';

This return the sum of gold to be 4130.
That's not enough.

**QUESTION:** Let's have a look at how much average gold people own, depending on their job.

SELECT job, SUM(inhabitant.gold), AVG(inhabitant.gold)
FROM inhabitant
GROUP BY job
ORDER BY AVG(inhabitant.gold);
job	SUM(inhabitant.gold)	AVG(inhabitant.gold)

farmer	10	10
?	70	70
merchant	250	250
blacksmith	390	390
weaponsmith	790	395
author	420	420
pilot	490	490
baker	1750	583.333333333333
smith	1250	625
dealer	2130	710
butcher	11370	2842.5

**QUESTION:** Very interesting: For some reason, butchers own the most gold. How much gold do different inhabitants have on average, depending on their state (friendly, ...)?

SELECT state, avg(gold)
FROM INHABITANT
GROUP BY state;

job	avg(gold)
?	70
evil	512.85714285714
friendly	706.363636363636
kidnapped	490

**QUESTION:** Ok, so the only way is to mug the villains. Or I might as well go ahead and just kill Dirty Dieter with my sword!

DELETE FROM INHABITANT
WHERE name = 'Dirty Dieter';

**QUESTION:** Heeeey! Now I'm very angry! What will you do next, Sriya?

DELETE FROM INHABITANT
WHERE name = 'Dirty Diane';

**QUESTION:** Yeah! Now I release the pilot!

UPDATE INHABITANT
SET state = 'friendly'
WHERE job = 'pilot';

Thank's for releasing me, Sriya! I will fly you home!

**QUESTION:** I take my sword, some gold and lots of useless items with me as a souvenir. What a big adventure!

UPDATE INHABITANT
SET state = 'emigrated'
WHERE personid = 20;
