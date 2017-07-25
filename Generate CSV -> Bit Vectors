import csv
import random
import pickle
from BitVector import *

def newCSVtoBV(baseName,numFiles,numColumns):
    header = []
    for i in range(numColumns):
        title = "Column " + str(i + 1)
        header.append(title)
    toCompare = []
    for j in range(numFiles):
        csvName = "C:\\Users\Brielle\Documents\internship\Tables\\" + str(baseName) + str(j) + ".csv"
        if j == 0:
            head = header
        else:
            head = random.sample(header, random.randint(0, len(header)))
        with open(csvName, "w", newline="") \
                as f:
            writer = csv.writer(f)
            writer.writerow(head)
            f.close()
        toCompare.append(str(csvName))
    universalColumns = {}
    for table in toCompare:
        with open(table, "r") as f:
            reader = csv.reader(f)
            k = next(reader)
        for column in k:
            if column not in universalColumns:
                universalColumns[column] = len(universalColumns)
    allBVs = []
    for table in toCompare:
        bv = BitVector(size=len(universalColumns))
        with open(table, "r") as f:
            reader = csv.reader(f)
            k = next(reader)
        for column in k:
            bv[universalColumns[column]] = 1
        allBVs.append(bv)
    pklName = "C:\\Users\Brielle\Documents\internship\Tables\\" + str(baseName) + ".pkl"
    output = open(pklName, 'wb')
    pickle.dump(allBVs, output)
    output.close()
    #with open(pklName, 'rb') as f:
    #    allBVs = pickle.load(f)
    return allBVs

bv = newCSVtoBV("t",1000,1000)

for i in bv:
    print(i)
