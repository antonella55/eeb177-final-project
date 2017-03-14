# Final Project - *Alligatoridae* Family

### Antonella Gonzalez

### Introduction
The *Alligatoridae* family is made up of species classified as alligators and caimans. Alligatoridae belongs to the order Crocodilia which is a sister taxa to birds(aves) and turtles(Testudines) due to the presence of the amniotic sac(2)(See Figures 1 and 2). All members of the Alligatoridae are carnivorous. Today the *Alligatoridae* family is distributed in Central and South America as well as the Southeastern United States (1). Their distinguished features such as massive skull and short, broad snout have not change significantly since the late Triassic. Members of *Alligatoridea* do not tolerate salt water, therefore their main way of dispersal was via land bridges. There is evidence that within *Alligatoridea* the caiman subclass evolved from the alligator subclass after its dispersal southward from North America to South America.

![Figure 1: Alligatoridae Family and sister taxa](Figures/alligatoridea-phylo2.png)

![Figure 2: Phylogeny of extant amniot tetrapod vertebrates](Figures/figure2.png)

### Background on the DataSet
My original data file from the pbdb website contained occurence data for specimens in the alligatoridae family and when using the the shell command: 

		tail -n +19 alligatoridae_pbdb_data.csv | cut -d "," -f7 | sort | wc -l

 I can see that there are 444 specimens in the data. Specimens were identified to the family, genus or species rank within my dataset. Therefore, previous functions that I have written were to be able to sort through each occurence data and select only data which was identified to the species rank. For example, to create a species ranges dictionary I used the following python code:

		species_ranges=defaultdict(list)
		for line in alligator:
			items = line.split('","')
			min_ma = round(float(items[15]),3)
			max_ma = round(float(items[14]),3)
			species_name = items[9]
			if re.search(r"species", line):
				species_ranges[species_name].append(str(min_ma))
				species_ranges[species_name].append(str(max_ma))
Using this dictionary, I wrote a csv file that contained only the genus, species, min_ma and max_ma for specimens identified to the species rank:

		output=open("alligatoridae_ranges.csv", "w") #i am making the output file
		for key, values in species_ranges.items():
			values.sort()
			#the largest value appears last in list and smallest appears first in list
			max_age = values[-1]
			min_age = values[0]
			genus=key.split(" ")[0] 
			outline= "{},{},{},{}\n".format(genus, key, min_age, max_age)
			print(outline)
			output.write(outline)

Which looked like this:
