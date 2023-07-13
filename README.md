Projeto realizado com auxilio de React.Js.Utilizando axios juntamente com o aplicação web Replit para o uso da API desenvolvida.
API:
var pessoas = [
  { "id": 1, "nome": "Luiz", "email": "luiz@gmail.com" },
  { "id": 2, "nome": "Fernando", "email": "Fernando@gmail.com" },
  { "id": 3, "nome": "Joao", "email": "joao@gmail.com" },
  { "id": 4, "nome": "Mario", "email": "mario@gmail.com" }
];

var http = require('http');

var server = http.createServer(function(request, response) {
  // Configurando os cabeçalhos CORS
  response.setHeader('Access-Control-Allow-Origin', '*');
  response.setHeader('Access-Control-Allow-Methods', 'GET, POST, PUT, DELETE, OPTIONS');
  response.setHeader('Access-Control-Allow-Headers', 'Content-Type');
  response.setHeader('Access-Control-Max-Age', '86400'); // 24 horas

  if (request.method === 'OPTIONS') {
    // Responde diretamente para requisições OPTIONS
    response.writeHead(200);
    response.end();
  } else if (request.method === 'POST') {
    // Recebe dados via POST e atualiza a variável "pessoas"
    var requestBody = '';
    request.on('data', function(data) {
      requestBody += data;
    });
    request.on('end', function() {
      // Aqui você pode processar o requestBody como desejar
      var newPerson = JSON.parse(requestBody);
      newPerson.id = pessoas.length + 1;
      pessoas.push(newPerson);

      // Responde com a lista atualizada de pessoas
      response.writeHead(200, { 'Content-Type': 'application/json' });
      response.write(JSON.stringify(pessoas));
      response.end();
    });
  } else if (request.method === 'DELETE') {
    // Recebe o ID da pessoa a ser excluída
    var url = request.url;
    var id = parseInt(url.substring(url.lastIndexOf('/') + 1));

    // Procura o índice da pessoa com o ID correspondente
    var index = -1;
    for (var i = 0; i < pessoas.length; i++) {
      if (pessoas[i].id === id) {
        index = i;
        break;
      }
    }

    if (index !== -1) {
      // Remove a pessoa da lista
      pessoas.splice(index, 1);
      response.writeHead(200);
      response.end();
    } else {
      // ID não encontrado, retorna erro 404
      response.writeHead(404);
      response.end();
    }
  } else {
    // Responde com os dados das pessoas para outras requisições
    response.writeHead(200, { 'Content-Type': 'application/json' });
    response.write(JSON.stringify(pessoas));
    response.end();
  }
});

server.listen(3000);
