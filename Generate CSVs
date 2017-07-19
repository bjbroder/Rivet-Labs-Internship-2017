import csv
import random
import string

def fill(lis,numCol):
    for j in range(len(lis)):
        for i in range(numCol):
            length = random.randint(1,10)
            val = ""
            for k in range(length):
                val+= random.choice(string.ascii_letters)
            lis[j].append(val)
    return lis

def createCSVs(baseName,numFiles,numColumns,numRows):
    header = []
    for i in range(numColumns):
        title = "Column " + str(i + 1)
        header.append(title)
    for i in range(numFiles):
        values = []
        for j in range(numRows):
            values.append([])
        with open(str(baseName) + str(i) + ".csv", "w", newline="") as f:
            writer = csv.writer(f)
            writer.writerow(header)
            writer.writerows(fill(values,numColumns))
            f.close()
