def connect(user, password, db, host='localhost', port=5432):
    url = 'postgresql://{}:{}@{}:{}/{}'
    url = url.format(user, password, host, port, db)

    con = sqlalchemy.create_engine(url, client_encoding='utf8')

    meta = sqlalchemy.MetaData(bind=con, reflect=True)

    return con, meta

con, meta = connect('postgres', 'sqlkey', 'test')

from sqlalchemy import create_engine
engine = create_engine('postgresql://localhost:5432/test')

def mean(table,column):
    rows = con.execute("select avg(" + column + ") as avg from " + table)
    for row in rows:
        return row.avg

def max(table,column):
    return con.execute("select max(" + column + ") from " + table)

def min(table,column):
    return con.execute("select min(" + column + ") from " + table)

def stddev(table,column):
    return con.execute("select stddev(" + column + ") from " + table)

def mode(table,column):
    return con.execute("select " + column + " from " + table + " group by " + column + " order by count(*) desc limit 1")

def median(table,column):
    return con.execute("select median(" + column + ") as median_value from " + table)

def stats(table, column):
    return max(table, column), min(table, column), mean(table, column), median(table, column), mode(table, column), \
    stddev(table, column)

def col(table, column):
    rows = con.execute("select " + column + " from " + table)
    lis = []
    for row in rows:
        print(row)
        lis.append(row)
    return lis
