# configdb
## 解决8.0安全问题
* alias mysql=/usr/local/mysql/bin/mysql
* mysql -u root -p
* use mysql;
* alter user 'root'@'localhost' identified with mysql_native_password by 'root123';


/**
 * 查询语句
 * @param sql
 * @param cb
 */
const query = function (sql, cb) {
    console.error('query ======>' + sql)
    client.getConnection(function (err, connection) {

        if (err) {
            console.error('mysql connnetion err =' + err)
            cb(err);
            // throw err;
        } else {
            connection.query(sql, function (connerr, result, fileds) {
                if (connerr) {
                    console.error('query err =====>' + connerr)
                    cb(err);
                } else {
                    cb(null, result);
                }
                connection.release(); //释放资源
            })
        }
    })
};

/**
 * 插入数据语句
 * @param table
 * @param data
 * @returns {string}
 */
const insertSql = function (table, data) {
    let sql = 'insert into ' + table;
    let keyStr = ' (';
    let valuesStr = 'values(';
    for (let i in data) {
        keyStr += i + ',';
        if ((typeof data[i]).indexOf("string") === 0) {
            valuesStr += "'" + data[i] + "'" + ',';
        } else {
            valuesStr += data[i] + ',';
        }

    }
    keyStr = keyStr.substring(0, keyStr.length - 1);
    valuesStr = valuesStr.substring(0, valuesStr.length - 1);
    keyStr += ') '
    valuesStr += (') ');
    sql += keyStr + valuesStr;
    return sql;
};

/**
 * 更新数据语句
 * @param mainKey
 * @param mainValue
 * @param table
 * @param data
 * @returns {string}
 */
const updateSql = function (mainKey, mainValue, table, data) {
    let sql = 'update ' + table + ' set ';
    for (let i in  data) {
        if ((typeof data[i]).indexOf('string') === 0) {
            sql += i + '=' + "'" + data[i] + "'" + ',';
        } else {
            sql += i + '=' + data[i] + ',';
        }

    }
    sql = sql.substring(0, sql.length - 1);
    sql += ' where ' + mainKey + '= ' + mainValue + ';';
    console.error('updateSql=======>' + sql);
    return sql;
}
