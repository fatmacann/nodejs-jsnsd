## Lab 3.1 - Deliver Data from a Library API
The labs-1 folder contains the following files:
●data.js
●package.json
●validate.js
The data.js file contains the following:

```
'use strict'
const { promisify } = require('util')
const { randomBytes } = require('crypto')
const timeout = promisify(setTimeout)
async function data () {
await timeout(50)
return randomBytes(10).toString('base64')
}
module.exports = data

```
The data.js file exports a function that returns a promise (an async function) that resolves to
a random BASE64 string. This function represents some kind of asynchronous data source.
The package.json file contains the following:

```
{
"name": "labs-1",
"scripts": {
"start": "echo \"TODO: SET THE START SCRIPT\" && exit 1"
}
}
```

Using any Node core library and/or web framework create an HTTP server that meets the
following criteria:

*Listens on localhost
*Listens on port 3000
*Responds to HTTP GET requests to / with data from the data function as exported
from the data.js
Responds with a 404 to GET requests to any other route
The package.json start script must contain a command to start the server.
Run the following command to check whether the created server meets the criteria:

```
node validate
```

If the server was correctly implemented, the output of this command should be as follows:
PASSED

********
 bu görev için Node.js'de localhost'ta ve 3000 portunda dinleyen bir HTTP sunucusu oluşturmamız gerekiyor. Bu sunucu, / yoluna gelen GET isteklerine data.js dosyasındaki data fonksiyonundan alınan verilerle yanıt verecek ve diğer tüm yollara yapılan GET isteklerine 404 hata kodu ile yanıt verecek. Ayrıca, sunucuyu başlatmak için package.json dosyasındaki start script'ini güncellememiz gerekecek.

İşte bunu başarmak için izlenecek adımlar:

HTTP Sunucusu Oluştur:
Node.js'in yerleşik http modülünü kullanarak sunucuyu oluşturacağız. Bu sunucu 3000 portunda dinleyecek ve belirtilen yönlendirme mantığını işleyecek.

/ İçin GET İsteklerini Yönet:
Sunucu, / yolunda bir GET isteği aldığında, data.js dosyasındaki data fonksiyonunu çağıracak ve sonuç olarak elde edilen veriyi yanıt olarak gönderecek.

Diğer GET İsteklerini Yönet:
Diğer tüm GET istekleri için sunucu 404 durum kodu ile yanıt verecek.

package.json Güncelle:
Sunucuyu başlatmak için package.json dosyasındaki start script'ini değiştir. Örneğin, node server.js komutunu kullanabiliriz (sunucu dosyamızın adı server.js varsayılarak).

Sunucu için kodu yazalım. server.js adında bir dosya oluşturalım:

```
'use strict';
const http = require('http');
const data = require('./data');

const server = http.createServer(async (req, res) => {
  if (req.url === '/' && req.method === 'GET') {
    try {
      const result = await data();
      res.writeHead(200, { 'Content-Type': 'text/plain' });
      res.end(result);
    } catch (error) {
      res.writeHead(500);
      res.end('Internal Server Error');
    }
  } else {
    res.writeHead(404);
    res.end('Not Found');
  }
});

server.listen(3000, 'localhost', () => {
  console.log('Server http://localhost:3000 adresinde dinleniyor');
});

```
Şimdi, package.json dosyasını güncelleyin:


```
{
  "name": "labs-1",
  "scripts": {
    "start": "node server.js"
  }
}
```
Sunucunuzu doğrulamak için belirtildiği gibi node validate komutunu çalıştırın. Bu komut, sunucunun belirtilen kriterleri karşılayıp karşılamadığını kontrol edecek. Her şey doğru bir şekilde ayarlandıysa, "PASSED" çıktısını görmelisiniz.



## Lab 3.2 - Implement a Status Code Response
The labs-2 folder contains the following files:

package.json
validate.js

The package.json file contains the following:

```
{
"name": "labs-2",
"scripts": {
"start": "echo \"TODO: SET THE START SCRIPT\" && exit 1"
}
}
```

Using any Node core library and/or web framework create an HTTP server that meets the
following criteria:

Listens on localhost
Listens on port 3000
Responds to HTTP GET requests to / with a 200 OK HTTP status, the content is
irrelevant
Responds to HTTP POST requests to / with a 405 Method Not Allowed HTTP status
The package.json start script must contain a command to start the server.
Run the following command to check whether the created server meets the criteria:

```
node validate
```
If the server was correctly implemented, the output of this command should be as follows:
PASSED

***

Bu görevde, Node.js kullanarak yerel bir HTTP sunucusu oluşturmanız ve bu sunucunun belirli kriterlere uygun şekilde çalışmasını sağlamanız gerekmektedir. İstenen kriterler şöyle:

Sunucu, yerel ağda (localhost) çalışmalı.
Sunucu, 3000 numaralı portu dinlemeli.
Sunucu, / yoluna yapılan HTTP GET isteklerine 200 OK durum kodu ile yanıt vermelidir. İçerik önemli değil.
Sunucu, / yoluna yapılan HTTP POST isteklerine 405 Method Not Allowed durum kodu ile yanıt vermelidir.
package.json dosyasındaki start betiği, sunucuyu başlatacak bir komut içermelidir.
Bu gereksinimleri karşılayacak bir sunucu oluşturmak için Node.js'in http modülünü kullanabilirsiniz. İşte bu gereksinimleri karşılayacak basit bir sunucu örneği:



```
const http = require('http');

const server = http.createServer((req, res) => {
  if (req.method === 'GET' && req.url === '/') {
    res.writeHead(200);
    res.end('Hello, World!');
  } else if (req.method === 'POST' && req.url === '/') {
    res.writeHead(405);
    res.end('Method Not Allowed');
  } else {
    res.writeHead(404);
    res.end('Not Found');
  }
});

server.listen(3000, 'localhost', () => {
  console.log('Server is running on http://localhost:3000');
});
```

Bu kod, yukarıdaki kriterleri karşılayacak şekilde yazılmıştır. Sunucuyu başlatmak için package.json dosyasındaki start betiğini aşağıdaki gibi güncelleyebilirsiniz:


```
{
  "name": "labs-2",
  "scripts": {
    "start": "node server.js"
  }
}
```

Bu durumda, server.js dosyası yukarıdaki sunucu kodunu içermelidir. Sunucuyu başlatmak için terminalde npm start komutunu çalıştırabilirsiniz.


## Lab 4.1 - Render a View
Using either Fastify or Express, create a project with a /me route. Render a view that uses the
layout.hbs that was created in this chapter to create a small profile page. The HTML content
is unimportant, just make sure to render a view.
The labs-1 folder contains a file named validate.js. Make sure the server that the /me route
was added to is running in another terminal, then with the current working directory set to the
labs-1 folder, run the following to check the implementation:

```
node validate
```

If successful the following output should be seen:
PASSED

***

Bu görevde, ya Fastify ya da Express kullanarak bir projede /me rotası oluşturmanız ve bu rotada bir profil sayfası görüntüleyen bir görünüm (view) render etmeniz gerekiyor. Ayrıca, bu görünümün bir layout.hbs dosyasını kullanarak oluşturulması gerekiyor. HTML içeriği önemli değil, önemli olan bir görünümün render edilmesi.

Öncelikle, Express veya Fastify ile bir projeyi başlatmanız ve gerekli bağımlılıkları yüklemeniz gerekmektedir. Burada Express kullanarak bir örnek yapacağım.

Adım 1: Proje Oluşturma ve Bağımlılıkları Yükleme
Yeni bir Node.js projesi oluşturun ve gerekli paketleri yükleyin. Örneğin, Express ve Handlebars için:



```
npm init -y
npm install express express-handlebars
```

Adım 2: Sunucu ve Görünümü Oluşturma
server.js adında bir dosya oluşturun ve aşağıdaki gibi kodlayın:


```
const express = require('express');
const exphbs = require('express-handlebars');

const app = express();

// Handlebars middleware ayarını yapın
app.engine('hbs', exphbs({ extname: '.hbs' }));
app.set('view engine', 'hbs');

// /me rotası için bir route oluşturun
app.get('/me', (req, res) => {
  res.render('profile', { layout: 'layout' });
});

// Sunucuyu 3000 portunda başlatın
app.listen(3000, () => {
  console.log('Server is running on http://localhost:3000');
});
```


Bu kod /me rotasında profile.hbs görünümünü layout.hbs layout'u ile render eder.

Adım 3: Görünüm Dosyalarını Oluşturma
views adında bir klasör oluşturun ve içine layout.hbs ve profile.hbs adında iki dosya yerleştirin.

layout.hbs örneği:


```
<!DOCTYPE html>
<html>
<head>
    <title>Profil Sayfası</title>
</head>
<body>
    {{{body}}}
</body>
</html>
```

profile.hbs örneği:


```
<h1>Merhaba, bu benim profilim!</h1>
```

Adım 4: Projeyi Çalıştırma ve Doğrulama
Sunucuyu başlatmak için terminalde node server.js komutunu çalıştırın. Ardından, labs-1 klasöründeki validate.js scriptini çalıştırarak projenizi doğrulayın.


```
node validate
```

Eğer her şey doğru yapıldıysa, "PASSED" çıktısını göreceksiniz.





## Lab 4.2 - Stream Some Content
The following code creates a stream with a built in delay when the stream function is called:

```
const { Readable, Transform } = require('stream')
function stream () {
const readable = Readable.from([
'this', 'is', 'a', 'stream', 'of', 'data'
].map((s) => s + '<br>'))
const delay = new Transform(({
transform (chunk, enc, cb) {
setTimeout(cb, 500, null, chunk)
}
}))
return readable.pipe(delay)
}

```
Using either Fastify or Express, create a new route at path /data and send the data from this
stream to the response when the /data route is requested.
The labs-2 folder contains a file named validate.js. Make sure the server that the /data
route was added to is running in another terminal, then with the current working directory set to
the labs-2 folder, run the following to check the implementation:
```
node validate
```

If successful the following output should be the result of this command:
PASSED

****

Bu görevde, Fastify veya Express kullanarak /data yolu altında bir rota oluşturmanız ve bu rotada, verilen kod örneğinde oluşturulan akışın (stream) içeriğini yanıt olarak göndermeniz isteniyor. Verilen kod örneği, belirli bir gecikme ile veri gönderen bir akış oluşturuyor. Bu akışı /data rotasında kullanarak istemciye göndermek gerekiyor.

Öncelikle, Express kullanarak bir örnek yapalım.

Adım 1: Proje Oluşturma ve Bağımlılıkları Yükleme
Yeni bir Node.js projesi oluşturun ve Express'i yükleyin.


```
npm init -y
npm install express
```

Adım 2: Sunucu ve Rota Oluşturma
server.js adında bir dosya oluşturun ve aşağıdaki gibi kodlayın:


```
const express = require('express');
const { Readable, Transform } = require('stream');

const app = express();

function stream() {
    const readable = Readable.from([
        'this', 'is', 'a', 'stream', 'of', 'data'
    ].map(s => s + '<br>'));

    const delay = new Transform({
        transform(chunk, enc, cb) {
            setTimeout(() => cb(null, chunk), 500);
        }
    });

    return readable.pipe(delay);
}

// /data rotası için bir route oluşturun
app.get('/data', (req, res) => {
    stream().pipe(res);
});

// Sunucuyu 3000 portunda başlatın
app.listen(3000, () => {
    console.log('Server is running on http://localhost:3000');
});
```

Bu kod, /data rotasında yukarıda verilen akışı kullanarak veriyi gönderir.

Adım 3: Projeyi Çalıştırma ve Doğrulama
Sunucuyu başlatmak için terminalde node server.js komutunu çalıştırın. Ardından, labs-2 klasöründeki validate.js scriptini çalıştırarak projenizi doğrulayın.


```
node validate
```

Eğer her şey doğru yapıldıysa, "PASSED" çıktısını göreceksiniz. Bu, oluşturduğunuz /data rotasının akışı doğru şekilde gönderdiğini ve belirtilen gereksinimleri karşıladığını gösterir.






## Lab 5.1 - Implement a RESTful JSON GET
The labs-1 folder contains the following files:

model.js
package.json
validate.js
The start field of the package.json file looks as follows:
"start": "echo \"Error: start script not specified\" && exit 1",
The model.js file contains the following:

```
'use strict'
module.exports = {
boat: boatModel()
}
function boatModel () {
const db = {
1: { brand: 'Chaparral', color: 'red' },
2: { brand: 'Chaparral', color: 'blue' }
}
return {
uid,
create,
read,
update,

delete: del
}
function uid () {
return Object.keys(db)
.sort((a, b) => a - b)
.map(Number)
.filter((n) => !isNaN(n))
.pop() + 1 + ''
}
function create (id, data, cb) {
if (db.hasOwnProperty(id)) {
const err = Error('resource exists')
err.code = 'E_RESOURCE_EXISTS'
setImmediate(() => cb(err))
return
}
db[id] = data
setImmediate(() => cb(null, id))
}
function read (id, cb) {
if (!(db.hasOwnProperty(id))) {
const err = Error('not found')
err.code = 'E_NOT_FOUND'
setImmediate(() => cb(err))
return
}
setImmediate(() => cb(null, db[id]))
}
function update (id, data, cb) {
if (!(db.hasOwnProperty(id))) {
const err = Error('not found')
err.code = 'E_NOT_FOUND'
setImmediate(() => cb(err))
return
}
db[id] = data
setImmediate(() => cb())
}

function del (id, cb) {
if (!(db.hasOwnProperty(id))) {
const err = Error('not found')
err.code = 'E_NOT_FOUND'
setImmediate(() => cb(err))
return
}
delete db[id]
setImmediate(() => cb())
}
}
```


Use either Fastify or Express to implement a RESTful HTTP server so that when the command
npm start is executed, a server is started that listens on process.env.PORT.
If implementing in Fastify, remember that running npm init fastify -- --integrate in
the labs-1 folder will set up the project npm start is executed the server will automatically
listen on process.env.PORT.
The server should support a GET request to a single route: /boat/{id} where {id} is a
placeholder for any given ID - for instance /boat/2.
The GET /boat/{id} route should respond with a JSON payload. The route should also
respond with the correct headers for a JSON response (Content-Type:
application/json).
The server should only support this GET route. That means that any other routes or any other
verbs should be handled according to the HTTP specification. Thankfully Express and Fastify
will do most of this for us.
The following cases must be successfully handled:

●A successful request should respond with a 200 status code. Express and Fastify do this
automatically.
●The response should have the correct mime type header. In this case we need to make
sure the Content-Type header is set to application/json.
●A GET request to a route that does not exist should respond with a 404 status code.
Fastify does this automatically and the typical Express configuration also handles this by
default.
●If a given boat ID isn't found in the model the server should respond with a 404 status
code. The response body can contain anything, but it's important that the response
status is set to 404.
●Unexpected errors in the model should cause the server to respond with a 500 status
code. This means that if the read method of the model passed an Error object to the
callback that was unexpected or unrecognized, that error needs to be propagated to the
framework we're using in some way so that the framework can automatically generate a
500 response.
●In the HTTP specification there is some ambiguity over how to handle unsupported
HTTP methods. Any HTTP method other than GET should be responded to with either a
400, 404 or 405 status code. Again Fastify and Express will respond to unsupported
methods with one of these status codes.

Do not edit the model.js file, it will be overwritten by the validation process anyway. The
model.js file is deliberately noisy, providing methods that we don't need for this exercise. This
reflects the philosophical approach of the certification to provide occasionally messy API's to
integrate with in order to better reflect real-world scenarios.
Once the server has been implemented, the following command can be executed to validate the
implementation:
```
node validate.js
```
When correctly implemented the result of this command should be as follows:
PASSED

***

Bu görevde, Fastify veya Express kullanarak bir RESTful HTTP sunucusu uygulamanız gerekiyor. Sunucu, npm start komutu çalıştırıldığında process.env.PORT üzerinde dinlemeli. Göreviniz, sadece bir GET rotasını (/boat/{id}) destekleyen bir sunucu oluşturmak.

İşte adım adım çözümü:

Proje Yapılandırması:

Fastify veya Express kullanarak başlayın.
package.json dosyasındaki start komutunu, sunucuyu başlatmak için düzenleyin. Örneğin Express için "start": "node app.js" şeklinde olabilir.
Sunucu Oluşturma ve Rota Ayarlama:

Bir HTTP sunucusu oluşturun ve process.env.PORT üzerinde dinlemeye başlayın.
/boat/{id} rotası için bir GET isteği yönlendiricisi (route handler) ekleyin. Bu rotada, id parametresi alınacak ve model.js içindeki uygun fonksiyon çağrılacak.
GET İsteklerini İşleme:

İstemci GET /boat/{id} rotasına istek yaptığında, id parametresine göre model.js'den uygun veriyi okuyun.
Eğer belirtilen ID'ye ait bir kayıt varsa, 200 durum kodu ile birlikte JSON formatında bir yanıt döndürün.
Belirtilen ID yoksa, 404 durum kodu ile yanıt verin.
Hata Yönetimi:

Modelde beklenmeyen bir hata oluşursa, 500 durum kodu ile yanıt verin.
Desteklenmeyen HTTP yöntemleri için 400, 404 veya 405 durum kodlarından biri ile yanıt verin.
Content-Type Ayarı:

Tüm yanıtlarınızın Content-Type başlığının application/json olarak ayarlandığından emin olun.
Test ve Doğrulama:

Sunucunuzu test edin ve çalıştığından emin olun.
node validate.js komutunu çalıştırarak uygulamanızı doğrulayın.
İşte Express kullanarak basit bir örnek kod:


```
const express = require('express');
const app = express();
const port = process.env.PORT || 3000;

const model = require('./model'); // model.js dosyasını dahil edin

app.get('/boat/:id', (req, res) => {
    model.boat.read(req.params.id, (err, data) => {
        if (err) {
            if (err.code === 'E_NOT_FOUND') {
                res.status(404).send('Not found');
            } else {
                res.status(500).send('Server error');
            }
        } else {
            res.status(200).json(data);
        }
    });
});

app.listen(port, () => {
    console.log(`Server is running on port ${port}`);
});

```

Bu kod, yukarıda belirtilen gereksinimleri karşılayacak temel bir yapı sunar. Fastify ile uygulamak isterseniz, benzer bir yapı kurabilirsiniz. Fastify daha performans odaklı olup, Express'e göre biraz farklı bir API kullanır.



## Lab 6.1 - Implement a RESTful JSON POST

The labs-1 folder contains the following files:


model.js
package.json
validate.js
The model.js file and the package.json file are exactly the same as the first lab exercise in
the previous chapter.
The model.js file has the following content:

```
'use strict'
module.exports = {
boat: boatModel()
}
function boatModel () {
const db = {
1: { brand: 'Chaparral', color: 'red' },
2: { brand: 'Chaparral', color: 'blue' }
}
return {
uid,
create,
read,
update,
del

}
function uid () {
return Object.keys(db)
.sort((a, b) => a - b)
.map(Number)
.filter((n) => !isNaN(n))
.pop() + 1 + ''
}
function create (id, data, cb) {
if (db.hasOwnProperty(id)) {
const err = Error('resource exists')
err.code = 'E_RESOURCE_EXISTS'
setImmediate(() => cb(err))
return
}
db[id] = data
setImmediate(() => cb(null, id))
}
function read (id, cb) {
if (!(db.hasOwnProperty(id))) {
const err = Error('not found')
err.code = 'E_NOT_FOUND'
setImmediate(() => cb(err))
return
}
setImmediate(() => cb(null, db[id]))
}
function update (id, data, cb) {
if (!(db.hasOwnProperty(id))) {
const err = Error('not found')
err.code = 'E_NOT_FOUND'
setImmediate(() => cb(err))
return
}
db[id] = data
setImmediate(() => cb())
}


function del (id, cb) {
if (!(db.hasOwnProperty(id))) {
const err = Error('not found')
err.code = 'E_NOT_FOUND'
setImmediate(() => cb(err))
return
}
delete db[id]
setImmediate(() => cb())
}
}

```


The package.json file looks as follows:

```
{
"name": "labs-1",
"version": "1.0.0",
"description": "",
"scripts": {
"start": "echo \"Error: start script not specified\" && exit 1",
"test": "echo \"Error: no test specified\" && exit 1"
},
"keywords": [],
"author": "",
"license": "ISC"
}

```

Use either Fastify or Express to implement a RESTful HTTP server so that when the command
npm start is executed it starts a server that listens on process.env.PORT.
The server should support a POST request to /boat that uses the model.js file to create a
new entry. The route should only accept application/json mime-type requests and should
respond with application/json content-type responses.
The POST request should expect JSON data to be sent in the following format:
```
{ data: { brand, color } }

```
A successful request should respond with a 201 Created status code. Unexpected errors should
result in a 500 Server Error response.
The service must also support the same GET /boat/{id} route as implemented in the
previous chapter.


It is not necessary to validate user input for this exercise.
Feel free to copy the files and folders from the labs-1 answer of the previous chapter into the
labs-1 folder of this chapter and then build upon, or else start from scratch, as preferred.
Making sure that the labs-1 folder is the current working directory, run the following command to
validate the completed exercise:

```
node validate
```
When correctly implemented, this command should output the following:
PASSED

****


Bu laboratuvar egzersizinde, Fastify veya Express kullanarak RESTful bir HTTP sunucusu uygulayacaksınız. Bu sunucu, model.js dosyasını kullanarak /boat adresine yapılan POST istekleriyle yeni girişler oluşturacak şekilde tasarlanmalıdır. POST isteği, belirli bir JSON formatında veri almalı ve başarılı bir istek durumunda 201 Oluşturuldu durum kodu ile yanıt vermelidir. Beklenmedik hatalar ise 500 Sunucu Hatası yanıtı ile sonuçlanmalıdır. Ayrıca, bu servis önceki bölümde uygulanan GET /boat/{id} rotasını da desteklemelidir.

İlk adım olarak, Fastify veya Express kullanarak temel bir HTTP sunucusu oluşturmanız gerekiyor. İşte adım adım bir çözüm önerisi:

Proje Bağımlılıklarını Kurma:

Fastify veya Express'i projenize ekleyin.
npm init komutuyla package.json dosyasını oluşturun veya mevcut olanı kullanın.
Fastify veya Express'i npm install fastify veya npm install express komutlarıyla kurun.
Sunucuyu Oluşturma ve Ayarlama:

Fastify veya Express ile bir HTTP sunucusu oluşturun.
Sunucunuzun process.env.PORT üzerinde dinlemesini sağlayın.
/boat adresine POST istekleri için bir rota oluşturun.
POST İsteği İşleyici:

POST isteği için bir işleyici fonksiyon yazın. Bu fonksiyon, gelen JSON verilerini almalı ve model.js dosyasındaki create fonksiyonunu kullanarak yeni bir giriş oluşturmalıdır.
Başarılı bir giriş oluşturulduğunda, 201 durum kodu ile yanıt verin.
GET İsteği İşleyici:

Önceki bölümde yapılan GET /boat/{id} rotası için bir işleyici fonksiyon ekleyin.
Hata Yönetimi:

Beklenmedik hatalar için 500 Sunucu Hatası durum kodu ile yanıt verin.
Sunucuyu Başlatma:

package.json dosyasında npm start komutunu, sunucuyu başlatan komutla güncelleyin. Örneğin: "start": "node server.js".
Doğrulama:

Tamamladığınızda, node validate komutunu çalıştırarak uygulamanızı test edin.
İşte bu adımları takip eden örnek bir kod parçası:


```
// Fastify veya Express modülünü dahil et
const fastify = require('fastify')({ logger: true });
// veya: const express = require('express');
// const app = express();

const { boat } = require('./model.js');

// POST rotası için işleyici
fastify.post('/boat', async (request, reply) => {
  try {
    const id = boat.uid();
    await boat.create(id, request.body.data);
    reply.status(201).send({ id });
  } catch (error) {
    reply.status(500).send(error);
  }
});

// GET rotası için işleyici
fastify.get('/boat/:id', async (request, reply) => {
  try {
    const data = await boat.read(request.params.id);
    reply.send(data);
  } catch (error) {
    reply.status(500).send(error);
  }
});

// Sunucuyu başlat
fastify.listen(process.env.PORT || 3000, (err, address) => {
  if (err) throw err;
  fastify.log.info(`Sunucu ${address} adresinde çalışıyor`);
});
```

Bu örnek Fastify kullanılarak yazılmıştır, ancak Express ile de benzer bir yapı kurabilirsiniz. Her iki durumda da, işlevsellikleri ve hata yönetimini özelleştirmeniz gerekebilir.




## Lab 6.2 - Implement a RESTful JSON DELETE
The labs-2 folder contains the following files:

model.js
package.json
validate.js

The model.js file and the package.json file are exactly the same as the first lab exercise in
this chapter and in the previous chapter.
The model.js file has the following content:

```
'use strict'
module.exports = {
boat: boatModel()
}
function boatModel () {
const db = {
1: { brand: 'Chaparral', color: 'red' },
2: { brand: 'Chaparral', color: 'blue' }
}
return {
uid,
create,
read,
update,
del
}


function uid () {
return Object.keys(db)
.sort((a, b) => a - b)
.map(Number)
.filter((n) => !isNaN(n))
.pop() + 1 + ''
}
function create (id, data, cb) {
if (db.hasOwnProperty(id)) {
const err = Error('resource exists')
err.code = 'E_RESOURCE_EXISTS'
setImmediate(() => cb(err))
return
}
db[id] = data
setImmediate(() => cb(null, id))
}
function read (id, cb) {
if (!(db.hasOwnProperty(id))) {
const err = Error('not found')
err.code = 'E_NOT_FOUND'
setImmediate(() => cb(err))
return
}
setImmediate(() => cb(null, db[id]))
}
function update (id, data, cb) {
if (!(db.hasOwnProperty(id))) {
const err = Error('not found')
err.code = 'E_NOT_FOUND'
setImmediate(() => cb(err))
return
}
db[id] = data
setImmediate(() => cb())
}
function del (id, cb) {
if (!(db.hasOwnProperty(id))) {



const err = Error('not found')
err.code = 'E_NOT_FOUND'
setImmediate(() => cb(err))
return
}
delete db[id]
setImmediate(() => cb())
}
}

```


The package.json file looks as follows:
```
{
"name": "labs-1",
"version": "1.0.0",
"description": "",
"scripts": {
"start": "echo \"Error: start script not specified\" && exit 1",
"test": "echo \"Error: no test specified\" && exit 1"
},
"keywords": [],
"author": "",
"license": "ISC"
}

```
Use either Fastify or Express to implement a RESTful HTTP server so that when the command
npm start is executed it starts a server that listens on process.env.PORT.
The server should support a DELETE request to /boat/{id} where {id} is a placeholder for
any given ID - for instance /boat/2.
A successful request should result in an empty response body with a 204 No Content status
code. If the specified ID does not exist, the response should have a 404 status code. Any
unexpected errors should result in a 500 Server Error response.
The service must also support the same GET /boat/{id} route as implemented in the
previous chapter.
Feel free to copy the files and folders from the labs-1 answer into this labs-2 answer or start
from scratch as preferred.
Making sure that the labs-2 folder is the current working directory, run the following command to
validate the completed exercise:

```
node validate
```

***

Bu laboratuvar egzersizinde, Fastify veya Express kullanarak bir RESTful HTTP sunucusu oluşturacaksınız. Bu sunucu, /boat/{id} adresine yapılan DELETE isteklerini işleyecek şekilde tasarlanmalıdır. Burada {id}, silinmek istenen tekne kaydının kimliğidir (örneğin, /boat/2). Başarılı bir istek, 204 No Content durum kodu ile boş bir yanıt gövdesi döndürmelidir. Belirtilen ID mevcut değilse, yanıt 404 durum koduna sahip olmalıdır. Beklenmedik hatalar ise 500 Sunucu Hatası durum kodu ile sonuçlanmalıdır. Ayrıca, bu servis önceki bölümde uygulanan GET /boat/{id} rotasını da desteklemelidir.

İşte bu görevi tamamlamak için izlenebilecek adımlar:

Sunucu Kurulumu:

Fastify veya Express kullanarak bir HTTP sunucusu oluşturun.
Sunucunuzun process.env.PORT üzerinde dinlemesini sağlayın.
DELETE İsteği İşleyici:

/boat/{id} adresine DELETE istekleri için bir rota oluşturun.
Gelen istekteki {id} değerini alarak model.js dosyasındaki del fonksiyonunu çağırın.
Eğer silme işlemi başarılı olursa, 204 No Content durum kodu ile yanıt verin.
Eğer belirtilen ID mevcut değilse, 404 durum kodu ile yanıt verin.
GET İsteği İşleyici:

Önceki bölümde yapılan GET /boat/{id} rotası için bir işleyici fonksiyon ekleyin.
Hata Yönetimi:

Beklenmedik hatalar için 500 Sunucu Hatası durum kodu ile yanıt verin.
Sunucuyu Başlatma:

package.json dosyasında npm start komutunu, sunucuyu başlatan komutla güncelleyin. Örneğin: "start": "node server.js".
Doğrulama:

Tamamladığınızda, node validate komutunu çalıştırarak uygulamanızı test edin.
İşte bu adımları takip eden örnek bir kod parçası:


```
// Fastify veya Express modülünü dahil et
const fastify = require('fastify')({ logger: true });
// veya: const express = require('express');
// const app = express();

const { boat } = require('./model.js');

// DELETE rotası için işleyici
fastify.delete('/boat/:id', async (request, reply) => {
  try {
    await boat.del(request.params.id);
    reply.status(204).send();
  } catch (error) {
    if (error.code === 'E_NOT_FOUND') {
      reply.status(404).send();
    } else {
      reply.status(500).send(error);
    }
  }
});

// GET rotası için işleyici
fastify.get('/boat/:id', async (request, reply) => {
  try {
    const data = await boat.read(request.params.id);
    reply.send(data);
  } catch (error) {
    reply.status(500).send(error);
  }
});

// Sunucuyu başlat
fastify.listen(process.env.PORT || 3000, (err, address) => {
  if (err) throw err;
  fastify.log.info(`Sunucu ${address} adresinde çalışıyor`);
});

```
Bu örnek Fastify kullanılarak yazılmıştır, ancak Express ile de benzer bir yapı kurabilirsiniz. Her iki durumda da, işlevsellikleri ve hata yönetimini özelleştirmeniz gerekebilir.





## Lab 7.1 - Implement a Data Aggregating Service
The labs-1 folder contains the following files:


package.json
boat-service.js
brand-service.js
validate.js

The package.json file has the following content:

```
{
"name": "labs-1",
"version": "1.0.0",
"scripts": {
"test": "echo \"Error: no test specified\" && exit 1",
"start": "echo \"Error: start script not specified\" && exit 1"
},
"keywords": [],
"license": "UNLICENSED"
}
```

Create a service that is started when the npm start command is executed that consumes two
other HTTP services.
The service must bind to a port number defined by the PORT environment variable.
The services are provided with mock data in this project folder as boat-service.js and
brand-service.js.


When started, each mock service outputs a port. This output can be used to set the
BOAT_SERVICE_PORT and BRAND_SERVICE_PORT environment variables when starting the
aggregating service.
For instance if the port of the Boat service is 3333 and the port of the Brand service is 3334 the
server can be started like so:

```
PORT=3000 BOAT_SERVICE_PORT=3333 BRAND_SERVICE_PORT=3334 npm start
```

Be sure to use the BOAT_SERVICE_PORT and BRAND_SERVICE_PORT environment variables
in the service to get the relevant port for each service. For example, the values of these
environment variables could be loaded into the service implementation like so:

```
const {
BOAT_SERVICE_PORT,
BRAND_SERVICE_PORT
} = process.env
```

To make a request to the Brand service:
```
http://localhost:[BRAND_SERVICE_PORT]/[id]
```

The Boat service responds with JSON data in the following format:

```
{
"id": Number,
"brand": Number,
"color": String
}
```

A request to the Boat service: http://localhost:[BOAT_SERVICE_PORT]/[id]
The id and brand properties will only ever be Integers.
The Brand service responds with JSON data in the following format:

```
{
"id": Number,
"name": String
}

```
The brand property of the Boat service output corresponds to the id of the Brand service
entities.
Create a service which accepts GET requests at http://localhost:[PORT]/[id]. Use the
incoming id from the GET request to make a request to the Boat service and use data from the
Boat service to make a request to the Brand service to retrieve associated brand data.
Combine the information from the two responses into a JSON payload and send that as a
response. The JSON payload should have the following form:

```
{
"id": Number,
"color": String,
"brand": String
}
```

The aggregating service should handle various scenarios in the following ways:

●For a normal successful request, respond with a 200 status code and the JSON payload
of combined data ({id, color, brand}) as the response body. The Content-Type
header must be application/json.
●Respond with a 404 status code if either service responds with a 404 status.
●If either service is not available, respond with a 500 status code with any response body.
●If either service responds with a non-200 status code then respond with a 500 status
code with any response body.
●If a request is made to the aggregating service with an ID that is not a valid integer,
respond with a 400 status code, the response body is unimportant and can be anything.
●If either service responds with a 4XX status code that is not a 400 or 404 (401-403,
405-499) status code, then respond with a 500 status code with any response body.
●Be sure that if an upstream service is not available, that the service responds within
1250ms.

The validator code for this exercise starts the services automatically. Make sure that the
services are not running and then run the following command in the labs-1 folder to check the
implementation:

```
node validate.js
```

If the aggregating service is successfully implemented this should result in the following output:
PASSED


****

Bu laboratuvar egzersizinde, iki farklı HTTP servisini tüketen bir veri toplayıcı (aggregating) servis oluşturacaksınız. Bu servis, npm start komutu ile başlatılacak ve PORT çevre değişkeni tarafından belirlenen bir port numarasında çalışacak. Bu projede, boat-service.js ve brand-service.js dosyaları olarak sağlanan iki mock servis bulunmakta.

Bu servisler başladığında, hangi portta çalıştıklarını çıktı olarak verecekler. Bu çıktı, toplayıcı servisi başlatırken BOAT_SERVICE_PORT ve BRAND_SERVICE_PORT çevre değişkenlerini ayarlamak için kullanılabilir. Örneğin, Tekne servisinin portu 3333 ve Marka servisinin portu 3334 ise, sunucu şu şekilde başlatılabilir:


```
PORT=3000 BOAT_SERVICE_PORT=3333 BRAND_SERVICE_PORT=3334 npm start
```

Toplayıcı servisiniz, BOAT_SERVICE_PORT ve BRAND_SERVICE_PORT çevre değişkenlerini kullanarak her iki servisin port numaralarını almalıdır.

İşte bu görevi tamamlamak için yapmanız gereken adımlar:

Sunucu Kurulumu ve Rotaların Ayarlanması:

Fastify veya Express kullanarak bir HTTP sunucusu oluşturun.
Sunucunuzun process.env.PORT üzerinde dinlemesini sağlayın.
GET /:id rotası için bir işleyici fonksiyon oluşturun.
Tekne ve Marka Servislerine İstekler Yapma:

Gelen GET isteğindeki id değerini kullanarak, önce Tekne servisine (http://localhost:[BOAT_SERVICE_PORT]/[id]) ve ardından alınan brand değerini kullanarak Marka servisine (http://localhost:[BRAND_SERVICE_PORT]/[brand_id]) istek yapın.
Her iki servisten alınan verileri birleştirin.
Yanıt Oluşturma:

İki servisten gelen bilgileri birleştirerek {id, color, brand} formatında bir JSON yanıtı gönderin.
Hata Yönetimi:

Eğer herhangi bir servis 404 yanıtı verirse, toplayıcı servis de 404 durum kodu ile yanıt vermeli.
Herhangi bir servis mevcut değilse veya 200 dışında bir durum kodu verirse, 500 Sunucu Hatası durum kodu ile yanıt verin.
Geçersiz bir id ile istek yapılırsa, 400 durum kodu ile yanıt verin.
400 veya 404 dışında 4XX durum kodları için de 500 Sunucu Hatası durum kodu ile yanıt verin.
Eğer bir üst akış servisi mevcut değilse, servisiniz 1250ms içinde yanıt vermelidir.
Doğrulama:

Tamamladığınızda, node validate.js komutunu çalıştırarak uygulamanızı test edin.
İşte bu adımları takip eden örnek bir kod parçası:


```
// Fastify veya Express modülünü dahil et
const fastify = require('fastify')({ logger: true });
// veya: const express = require('express');
// const app = express();

const fetch = require('node-fetch');

const {
  BOAT_SERVICE_PORT,
  BRAND_SERVICE_PORT
} = process.env;

// GET rotası için işleyici
fastify.get('/:id', async (request, reply) => {
  const { id } = request.params;
  try {
    // Tekne servisine istek yap
    const boatResponse = await fetch(`http://localhost:${BOAT_SERVICE_PORT}/${id}`);
    if (!boatResponse.ok) throw new Error('Boat service error');

    const boatData = await boatResponse.json();

    // Marka servisine istek yap
    const brandResponse = await fetch(`http://localhost

```



## Lab 8.1 - Implement an HTTP Route-Based Proxy
The labs-1 folder contains the following files:

package.json
validate.js
The package.json file has the following content:

```
{
"name": "labs-1",
"version": "1.0.0",
"scripts": {
"test": "echo \"Error: no test specified\" && exit 1",
"start": "echo \"Error: start script not specified\" && exit 1"
},
"keywords": [],
"license": "UNLICENSED"
}
```

Create an HTTP service that initializes when npm start is executed and listens on whatever
the PORT environment variable is set to.
The service must be a transparent reverse HTTP proxy server such that a request to
http://localhost:{PORT}/?url={URL} will respond with:
1. the status code of {URL}
2. the headers provided at {URL}
3. the contents of the body at {URL}

The {URL} will only ever hold HTTP URLs, there's no need to proxy HTTPS URLs.
The service must meet the following conditions:

A request to any route other than / should respond with an HTTP Not Found response.
A request to / without a url query-string parameter should result in a Bad Request
HTTP response.
The proxy only needs to support HTTP GET requests.
Run the following command to check whether the implementation was successful:

```
node validate.js
```
When successfully implemented the output should be similar to the following:
PASSED


***
Bu laboratuvar egzersizinde, npm start komutu ile başlatıldığında belirli bir portta dinleyen ve HTTP isteklerini başka bir adrese yönlendiren (proxy) bir HTTP servisi oluşturmanız gerekmektedir. Bu servis, http://localhost:{PORT}/?url={URL} formatındaki bir isteği aldığında, belirtilen {URL}'nin durum kodunu, başlıklarını ve gövde içeriğini geri döndürecek şekilde tasarlanmalıdır. Burada {URL}, yalnızca HTTP URL'lerini içerecek; HTTPS URL'lerini proxy'lemek gerekmemektedir.

Servisin karşılaması gereken koşullar şunlardır:

/ dışındaki herhangi bir rotaya yapılan istek, HTTP Not Found (Bulunamadı) yanıtıyla cevaplanmalıdır.
url sorgu dizesi parametresi olmadan / adresine yapılan bir istek, Bad Request (Kötü İstek) HTTP yanıtıyla cevaplanmalıdır.
Proxy'nin yalnızca HTTP GET isteklerini desteklemesi gerekmektedir.
Uygulamanızı doğrulamak için node validate.js komutunu çalıştırabilirsiniz. Başarılı bir şekilde uygulandığında çıktı "PASSED" olmalıdır.

İşte bu görevi tamamlamak için izlenebilecek adımlar:

Sunucu Kurulumu:

Fastify veya Express kullanarak bir HTTP sunucusu oluşturun.
Sunucunuzun process.env.PORT üzerinde dinlemesini sağlayın.
Ana Rota İşleyicisi:

/ rotası için bir işleyici fonksiyon oluşturun.
Gelen istekte url sorgu dizesi parametresini kontrol edin.
Eğer url parametresi yoksa veya geçersizse, Bad Request yanıtı döndürün.
Proxy İşlevselliği:

Geçerli bir url alındığında, bu URL'ye bir GET isteği yapın.
Gelen yanıtın durum kodunu, başlıklarını ve gövde içeriğini proxy ederek geri döndürün.
Hata Yönetimi:

Eğer / dışında bir rota çağrılırsa, Not Found yanıtı döndürün.
İşte bu adımları takip eden örnek bir kod parçası:


```
// Fastify veya Express modülünü dahil et
const fastify = require('fastify')({ logger: true });
// veya: const express = require('express');
// const app = express();

const fetch = require('node-fetch');

// Ana rota için işleyici
fastify.get('/', async (request, reply) => {
  const { url } = request.query;
  if (!url) {
    return reply.status(400).send('Bad Request');
  }

  try {
    const response = await fetch(url);
    reply.status(response.status).headers(response.headers.raw()).send(response.body);
  } catch (error) {
    reply.status(500).send('Server Error');
  }
});

// Diğer rotalar için işleyici
fastify.get('*', (request, reply) => {
  reply.status(404).send('Not Found');
});

// Sunucuyu başlat
fastify.listen(process.env.PORT || 3000, (err, address) => {
  if (err) throw err;
  fastify.log.info(`Sunucu ${address} adresinde çalışıyor`);
});
```

Bu örnekte Fastify kullanılmıştır, ancak Express ile de benzer bir yapı kurabilirsiniz. Her iki durumda da, istek yönlendirme mantığını ve hata yönetimini özelleştirmeniz gerekebilir.



##  Lab 8.2 - Implement a Full Proxying Service
The labs-2 folder is completely empty. Create a service that listens on port 3000.
The service must proxy all requests/responses to https://jsonplaceholder.typicode.com.

Use the following command to check whether the implementation was successful:

```
node -e "http.get('http://localhost:3000/todos/1', (res) =>
res.pipe(process.stdout))"
```

This should output something similar to the following:
PASSED

***

Lab 8.2'de, port 3000'de dinleyen ve tüm istek/yanıtları https://jsonplaceholder.typicode.com adresine yönlendiren tam bir proxy (vekil sunucu) servisi oluşturmanız isteniyor. Bu, gelen istekleri alıp başka bir sunucuya yönlendiren ve sonra da o sunucunun yanıtlarını istemciye geri gönderen bir ara katman görevi görür.

İşte bu görevi yerine getirmek için gereken adımlar:

Node.js Sunucusu Oluşturma: İlk olarak, Node.js kullanarak basit bir sunucu oluşturmanız gerekiyor. Bu sunucu, gelen istekleri alacak ve başka bir sunucuya yönlendirecek.

Proxy İşlevselliği Ekleme: Sunucunuz, gelen istekleri https://jsonplaceholder.typicode.com adresine yönlendirmeli. Bu işlem için http-proxy gibi Node.js modüllerini kullanabilirsiniz.

Port 3000'de Dinleme: Sunucunuzun 3000 numaralı portta dinlemesi gerekiyor.

Test Etme: Sunucunuzun doğru çalışıp çalışmadığını kontrol etmek için, verilen Node.js komutunu kullanabilirsiniz. Bu komut, proxy sunucunuz aracılığıyla jsonplaceholder.typicode.com üzerinden bir TODO öğesini getirir ve çıktıyı gösterir.

Örnek Kod
İşte bu görevi yerine getirmek için kullanabileceğiniz basit bir Node.js sunucusu örneği:


```
const http = require('http');
const httpProxy = require('http-proxy');

// Proxy sunucusu oluştur
const proxy = httpProxy.createProxyServer({});

// Sunucuyu oluştur ve 3000 portunda dinlemeye başla
const server = http.createServer((req, res) => {
    // İstekleri jsonplaceholder.typicode.com'a yönlendir
    proxy.web(req, res, { target: 'https://jsonplaceholder.typicode.com' });
});

server.listen(3000, () => {
    console.log('Proxy sunucusu port 3000 üzerinde çalışıyor');
});

```
Bu kodu çalıştırmak için öncelikle http-proxy modülünü yüklemeniz gerekecek:


```
npm install http-proxy
```

Daha sonra yukarıdaki kodu bir dosyaya yapıştırın (örneğin, proxyServer.js) ve bu dosyayı Node.js ile çalıştırın. Ardından, verilen test komutunu kullanarak sunucunuzu test edebilirsiniz.




## Lab 9.1 - Implement a Service That Is Not Vulnerable to Parameter Pollution

The labs-1 folder contains the following files:

package.json
app.js
validate.js

The package.json file contains the following:

```
{
"name": "labs-1",
"version": "1.0.0",
"scripts": {
"start": "node app.js"
},
"license": "UNLICENSED",
"dependencies": {
"express": "^4.17.1"
}
}

```
Note that Express is a dependency of the project. Install the project dependency with the
following command, executed within the labs-1 folder:

```
npm install
```

The app.js file contains the following:

```
'use strict'
const express = require('express')
const app = express()
const router = express.Router()
const { PORT = 3000 } = process.env
router.get('/', (req, res) => {
setTimeout(() => {
res.send((req.query.un || '').toUpperCase())
}, 1000)
})
app.use(router)
app.listen(PORT, () => {
console.log(`Express server listening on ${PORT}`)
})
```

This is a small Express service that uppercases any input sent via a un query string parameter,
but it waits one second before sending the response.

This service is vulnerable to parameter pollution. A URL such as
http://localhost:3000/?un=a&un=b will cause the service to crash, assuming the service is
listening on port 3000.

Fix it, without changing any of the current functionality.

The parameter pollution attack may be handled as seen fit. For instance upper casing all forms,
or sending a 400 Bad Request, or any kind of response. The only thing that must not happen is
the service crashing and requests containing query-strings with a single un parameter must
continue to respond with the uppercased version of that value.

Run the validate.js file as follows, to validate the fix:

```
node validate.js
```

If successful this should output something similar to the following:
PASSED


***

Lab 9.1'de, verilen Express.js servisinin parametre kirliliğine karşı savunmasız olduğunu ve bu sorunu çözmeniz gerektiğini anlatıyor. Parametre kirliliği, bir web uygulamasına birden fazla aynı isimli parametre gönderildiğinde meydana gelebilir. Bu durumda, app.js dosyasındaki Express servisi, un adlı sorgu parametresinin birden fazla olması durumunda çökmektedir. Bu sorunu çözmek için, gelen isteklerdeki un parametresinin sadece ilk değerini kullanarak diğer değerleri yok saymanız veya bir hata mesajı göndermeniz gerekmektedir.

Çözüm Adımları
Kontrol Mekanizması Ekleme: app.js dosyasındaki router tanımına, un parametresinin birden fazla olup olmadığını kontrol eden bir mekanizma ekleyin. Eğer birden fazla ise, istemciye bir hata mesajı gönderin veya sadece ilk değeri kullanın.

Değişiklikleri Yapma: Aşağıdaki kod örneğinde, un parametresi için sadece ilk değerin kullanıldığı bir kontrol mekanizması eklenmiştir.

Güncellenmiş app.js Örneği

```
'use strict'
const express = require('express')
const app = express()
const router = express.Router()
const { PORT = 3000 } = process.env

router.get('/', (req, res) => {
    // 'un' parametresinin birden fazla olup olmadığını kontrol et
    if (Array.isArray(req.query.un)) {
        // Hata mesajı gönder
        res.status(400).send('Invalid request');
    } else {
        setTimeout(() => {
            res.send((req.query.un || '').toUpperCase())
        }, 1000)
    }
})

app.use(router)

app.listen(PORT, () => {
    console.log(`Express server listening on ${PORT}`)
})

```


Bu kodda, un parametresinin bir dizi olup olmadığını kontrol ediyoruz (Array.isArray(req.query.un)). Eğer dizi ise, yani birden fazla un parametresi varsa, 400 durum kodu ile bir hata mesajı gönderiyoruz. Eğer dizi değilse, yani sadece bir un parametresi varsa, işlevselliği aynı şekilde sürdürüyoruz.

Bu değişiklikleri yaptıktan sonra, node validate.js komutu ile çözümünüzü test edebilirsiniz. Başarılı bir şekilde tamamlanmışsa, "PASSED" mesajını göreceksiniz.



## Lab 9.2 - Validate a POST Request

The labs-2 folder contains a Fastify service. Other than the typical Fastify directory structure, it
has a model.js file, a routes/boat/index.js, a package-lock.json file and a
validate.js file.

The model.js file is the same as from the labs of Chapters 5 and 6.

The routes/boat/index.js file supports the following routes:

POST /boat
GET /boat/{id}
DELETE /boat/{id}

Apply validation to the POST route request body so that any POST request bodies that do not
have the shape { data: { brand, color }} are rejected with a 400 Bad Request status
code. Additional properties are allowed, but should be stripped before being stored.
Do not remove or otherwise modify any of the routes.
Remember to run npm install in the labs-2 folder to install project dependencies.
Run the validate.js file as follows, to validate that the route validation was correctly
implemented:

```
node validate.js
```

If successful this should output something similar to the following:
PASSED

***

Lab 9.2'de, Fastify kullanarak oluşturulmuş bir serviste POST isteklerini doğrulamanız gerekiyor. İsteklerin doğru yapıda olduğundan emin olmak için, gelen POST istek gövdelerinin belirli bir yapıya sahip olmasını sağlamalısınız. Belirtilen yapı { data: { brand, color }} şeklinde olmalı ve bu yapıya uymayan istekler 400 Bad Request durum kodu ile reddedilmelidir. Ek özellikler izinli olsa da, saklanmadan önce çıkarılmalıdır.

Adımlar
Doğrulama Şeması Oluşturma: Fastify, istekleri doğrulamak için JSON Schema'yı kullanır. Bu durumda, brand ve color özelliklerini içeren bir şema tanımlamanız gerekiyor.

Doğrulamayı Uygulama: routes/boat/index.js dosyasında POST yoluna bu doğrulama şemasını uygulayın.

Ek Özellikleri Kaldırma: İstek gövdesinde izin verilen özellikler dışında kalanları kaldırın.

Test Etme: Değişikliklerinizi yaptıktan sonra, node validate.js komutunu çalıştırarak doğrulamanın doğru şekilde yapıldığını kontrol edin.

Örnek Kod
Fastify'de istek gövdesini doğrulamak için şema tanımlamanız gerekecek. İşte routes/boat/index.js dosyası için bir örnek:


```
const boatSchema = {
  body: {
    type: 'object',
    required: ['data'],
    properties: {
      data: {
        type: 'object',
        required: ['brand', 'color'],
        properties: {
          brand: { type: 'string' },
          color: { type: 'string' }
        },
        additionalProperties: false
      }
    },
    additionalProperties: false
  }
}

fastify.post('/boat', { schema: boatSchema }, async (request, reply) => {
  // Burada istek gövdesini işleyin
})

```


Bu kod, POST isteğinin gövdesinin { data: { brand, color }} yapısına uygun olup olmadığını kontrol eder. Eğer yapıya uymazsa, Fastify otomatik olarak 400 Bad Request durum kodu ile yanıt verir.

Yapmanız gereken, bu şema tanımını routes/boat/index.js dosyasına eklemek ve uygun şekilde POST yoluna uygulamak. Ardından, node validate.js komutunu çalıştırarak doğrulamanın başarılı olup olmadığını kontrol edebilirsiniz.



## Lab 10.1 - Block an Attackers IP Address with Express
The labs-1 folder contains an Express application along with a validate.js file.
Imagine this is a deployed service, which is receiving a DoS attack from the IP address
111.34.55.211.
Edit the service so that this IP address, and only this IP address, receives a 403 Forbidden
response from the service.
Execute the following command to check the mitigation step worked:

```
node validate.js
```

If successful, output similar to the following should be seen:
PASSED

***

Lab 10.1'de, Express uygulamanızı belirli bir IP adresinden (111.34.55.211) gelen trafiği engelleyecek şekilde düzenlemeniz gerekiyor. Bu IP adresinden gelen isteklere 403 Forbidden (Yasak) yanıtı verilmesi isteniyor. Bu, bir tür DoS (Hizmet Reddi) saldırısına karşı bir önlem olarak düşünülebilir.

Çözüm Adımları
IP Adresini Kontrol Etme: İlk adım, gelen her isteğin IP adresini kontrol etmek. Express'te, istek nesnesi (req) üzerinden req.ip veya req.connection.remoteAddress kullanarak bir isteğin IP adresini alabilirsiniz.

Engellenen IP Adresine Yanıt Verme: Belirli bir IP adresi (bu durumda 111.34.55.211) tespit edildiğinde, isteğe 403 Forbidden yanıtı vermelisiniz.

Test Etme: Değişikliklerinizi yaptıktan sonra, node validate.js komutunu çalıştırarak çözümünüzü test edin.

Örnek Kod
İşte Express uygulamanızda yapmanız gereken değişikliklere bir örnek:


```
const express = require('express');
const app = express();
const BLOCKED_IP = '111.34.55.211';

app.use((req, res, next) => {
    if (req.ip === BLOCKED_IP) {
        res.status(403).send('Erişim Yasaklandı');
    } else {
        next();
    }
});

// Diğer route tanımlamalarınız burada olacak

app.listen(3000, () => {
    console.log('Sunucu port 3000 üzerinde çalışıyor');
});
```

Bu kod, gelen her isteğin IP adresini kontrol eder ve eğer bu IP, engellenmiş IP'ye eşitse, 403 Forbidden yanıtı verir. Aksi takdirde, normal işleyişe (next()) devam eder.

Bu kodu uygulamanıza ekleyin ve node validate.js komutu ile çözümünüzü test edin. Başarılı bir şekilde tamamlanmışsa, "PASSED" mesajını göreceksiniz.




## Lab 10.2 - Block an Attackers IP Address with Fastify
The labs-2 folder contains a Fastify application along with a validate.js file.
Imagine this is a deployed service, which is receiving a DoS attack from the IP address
211.133.33.113.
Edit the service so that this IP address, and only this IP address, receives a 403 Forbidden
response from the service.
Execute the following command to check the mitigation step worked:

```
node validate.js
```

If successful, output similar to the following should be seen:
PASSED


***

Lab 10.2'de, Fastify uygulamanızı belirli bir IP adresinden (211.133.33.113) gelen trafiği engelleyecek şekilde düzenlemeniz gerekiyor. Bu IP adresinden gelen isteklere 403 Forbidden (Yasak) yanıtı verilmesi isteniyor. Bu, bir DoS (Hizmet Reddi) saldırısına karşı bir önlem olarak düşünülebilir.

Çözüm Adımları
IP Adresini Kontrol Etme: İlk adım, gelen her isteğin IP adresini kontrol etmek. Fastify'de, istek nesnesi (request) üzerinden request.ip kullanarak bir isteğin IP adresini alabilirsiniz.

Engellenen IP Adresine Yanıt Verme: Belirli bir IP adresi (bu durumda 211.133.33.113) tespit edildiğinde, isteğe 403 Forbidden yanıtı vermelisiniz.

Test Etme: Değişikliklerinizi yaptıktan sonra, node validate.js komutunu çalıştırarak çözümünüzü test edin.

Örnek Kod
İşte Fastify uygulamanızda yapmanız gereken değişikliklere bir örnek:


```
const fastify = require('fastify')({ logger: true });
const BLOCKED_IP = '211.133.33.113';

fastify.addHook('preHandler', async (request, reply) => {
    if (request.ip === BLOCKED_IP) {
        reply.code(403).send('Erişim Yasaklandı');
        return;
    }
});

// Diğer route tanımlamalarınız ve sunucu ayarlarınız burada olacak

fastify.listen(3000, err => {
    if (err) throw err;
    console.log('Sunucu port 3000 üzerinde çalışıyor');
});
```

Bu kod, gelen her isteğin IP adresini kontrol eder ve eğer bu IP, engellenmiş IP'ye eşitse, 403 Forbidden yanıtı verir. Aksi takdirde, normal işleyişe devam eder.

Bu kodu uygulamanıza ekleyin ve node validate.js komutu ile çözümünüzü test edin. Başarılı bir şekilde tamamlanmışsa, "PASSED" mesajını göreceksiniz.







