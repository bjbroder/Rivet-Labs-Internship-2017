import numpy as np
import pandas as pd
import scipy.stats as st
import math

def prepTable (table):
    df = pd.read_csv(table)
    df = df.dropna(axis=1, how='all')
    data = {}
    for column in df:
        col = []
        counter = 0
        df[column] = df[column].apply(pd.to_numeric, errors='ignore')
        for row in range(len(df[column])):
            if (type(df.loc[row][column]) == np.float64 or type(df.loc[row][column]) == np.float) \
                    and not math.isnan(df.loc[row][column]):
                col.append(df.loc[row][column])
            else:
                counter += 1
        if len(col) > 0:
            if (counter/len(col)) * 100 < 10:
                data[column] = col
    return pd.DataFrame.from_dict(data)

def compare(one, two, same, different, funky):
    for column in one:
        if column in two:
            stats, pval = st.ttest_ind(one[column].astype(float), two[column].astype(float), equal_var = False)
            if pval >= .05:
                if column in funky:
                    funky[column] = [funky[column][0]+1,funky[column][1]]
                elif column not in different:
                    if column not in same:
                        same[column] = [1,0]
                    else:
                        same[column] = [same[column][0] + 1, 0]
                else:
                    funky[column] = (1,different[column][1])
                    del different[column]
            else:
                if column in funky:
                    funky[column] = [funky[column][0],funky[column][1]+1]
                elif column not in same:
                    if column not in different:
                        different[column] = [0,1]
                    else:
                        different[column] = [0,different[column][1] + 1]
                else:
                    funky[column] = [same[column][0],1]
                    del same[column]
        else:
            print(str(column) + " is not in " + str(two))
    return same, different, funky

def checkTables(lis):
    prepped = []
    for i in lis:
        j = prepTable(i)
        print(str(i) + " has been prepped")
        prepped.append(j)
    same = {}
    different = {}
    funky = {}
    tables = []
    print()
    for i in range(len(prepped)-1):
        s,d,f = compare(prepped[i], prepped[i+1], same, different, funky)
        funky = f
        same = s
        different = d
        tables.append(lis[i])
    tables.append(lis[-1])
    return same, different, funky, tables

def main(tables, name):
    fout = open(name,"w")
    s,d,f,t = checkTables(tables)
    fout.write("Tables compared:\n")
    for i in t: fout.write("  > %s\n" % i)
    fout.write("\n\nColumns that appear similar over time:\n")
    for k in s: fout.write("  > %s\n" % k)
    fout.write("\n\nColumns that appear dissimilar over time:\n")
    for k in d: fout.write("  > %s\n" % k)
    fout.write("\n\nColumns that sometimes appear similar and sometimes appear dissimilar:\n")
    for k in f: fout.write("  > %s:%s\n" % (k,f[k]))
    fout.close
