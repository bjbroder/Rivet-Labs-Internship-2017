import csv
import pickle
from BitVector import *


def vectorize(name,amount):
    toCompare = []
    for i in range(amount):
        toCompare.append(str ("C:\\Users\Brielle\Documents\internship\Tables\\" + name + str(i) + ".csv"))
    universalColumns = {}
    for table in toCompare:
        with open(table, "r") as f:
            reader = csv.reader(f)
            i = next(reader)
        for column in i:
            if column not in universalColumns:
                universalColumns[column] = len(universalColumns)
    allBVs = []
    for table in toCompare:
        bv = BitVector( size = len(universalColumns))
        with open(table, "r") as f:
            reader = csv.reader(f)
            i = next(reader)
        for column in i:
            bv[universalColumns[column]] = 1
        allBVs.append(bv)
    output = open(name + '.pkl', 'wb')
    pickle.dump(allBVs, output)
    output.close()
    #with open(name + '.pkl', 'rb') as f:
    #    allBVs = pickle.load(f)
    return allBVs

#universalBV = BitVector( size = len(universalColumns))

print(vectorize("table",100))
