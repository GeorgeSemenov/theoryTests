- [[#Суммирование промисов]](лёгкая)
- [[#Замена метода массива groupBy]](лёгкая +)
- [[#asyncAuth]]
# Суммирование промисов
```ts
/*
Напиши функцию, которая принимает неограниченное 
количество промисов и возвращает их сумму
*/ 
// Запись Promise<number> - означает, что промис вернёт число

const sumProms = (...proms:Promise<number>[])=>{
    return Promise.all(proms).then(results=>results.reduce((sum,nxt) => sum+nxt,0))
}

const prom1:Promise<number> = new Promise ((resolve,reject)=>setTimeout(()=>resolve(5),100))
const prom2 = Promise.resolve(3)

sumProms(prom1,prom2).then(results=>console.log(results))
```

# Замена метода массива groupBy
```js
/*
Реализуй метод grouby, расширяющий стандартные методы массивов.
Метод должен возвращать сгруппированную версию массива - объект,
в котором каждый ключ является результатом выполнения переданной функции fn(arr[i]),
а значением - массивом, содержащим все элементы исходного массива с этим ключом
*/

const numbers = [6.1, 4.2, 6.3];
const arr = [{id:1},{id:2},{id:1}];

// сгруппируем числа по целой части
const fn = (item)=>Math.floor(item)
const nf = (item)=>item.id
/**
 * const grouped = numbers.groupby(fn) 
 * console.log(grouped)// [4: [4.2], 6: [6.1,6.3]]
 */

Array.prototype.groupBy = function(fn){
  const result = {};
  for (item of this){
    const key = fn(item)
    
    if(!result[key])
      result[key] = []

    result[key].push(item)
  }
  return result
}
// Используешь обычную функцию, а не стрелочную, потому что 
// 1. Стрелочная - потеряет this
// 2. у стрелочной нет args  и из неё нельзя сделать конструктор

console.log(numbers.groupBy(fn))
console.log(arr.groupBy(nf))
```

# asyncAuth
```js
/*
Функция asyncAuth(callback) принимает callback, в котором может быть передана ошибка в первом аргументе, и данные с бэкэнда во втором аргументе
asyncAuth((error,data)=>{})

Реализуй auth, которая вызывает asyncAuth, но возвращает промис

Нужно реализовать функцию tryAuth, которая принимает n - сколько максимльно раз можно вызвать auth, если же все вызовы потерпели неудачу, то должна возвращаться ошибка. 
*/

/*
Функция asyncAuth(callback) принимает callback, в котором может быть передана ошибка в первом аргументе, и данные с бэкэнда во втором аргументе
asyncAuth((error,data)=>{})

Реализуй auth, которая вызывает asyncAuth, но возвращает промис

Нужно реализовать функцию tryAuth, которая принимает n - сколько максимльно раз можно вызвать auth, если же все вызовы потерпели неудачу, то должна возвращаться ошибка. 
*/
async function asyncAuth(err,data){
  console.log(`data = ${data}, err = ${err}`)
}

const auth = ()=>{
  return Promise((resolve,reject)=>{
    asyncAuth((err,data)=>{
      if(err)
      {reject(err)}else{
        resolve(data)
      }
    })
  })
}

async function tryAuth(n){
  try{
    const res = await auth()
    return res;
  }catch(err){
    if(n>0){
      return tryAuth(n-1)
    }else{
      throw err
    }
  }
}
```