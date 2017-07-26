import pickle

def compareBVs (pklName):
    with open(pklName, 'rb') as f:
        allBVs = pickle.load(f)
    similar = []
    for i in range(len(allBVs) - 1):
        x = allBVs[i]
        for j in range(i + 1, len(allBVs)):
            y = allBVs[j]
            if x.count_bits() != 0 or y.count_bits() != 0:
                similarity = x.jaccard_similarity(y)
                if similarity >= .9:
                    similar.append((allBVs[i], allBVs[j]))
    return similar
