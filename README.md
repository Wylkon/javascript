# Airbnb JavaScript Style Guide() {

*A mostly reasonable approach to JavaScript*


## <a name='TOC'>Índice</a>

  1. [Tipos](#tipos)
  1. [Objetos](#objetos)
  1. [Arrays](#arrays)
  1. [Strings](#strings)
  1. [Funções](#funcoes)
  1. [Propriedades](#propriedades)
  1. [Variáveis](#variaveis)
  1. [Hoisting](#hoisting)
  1. [Conditional Expressions & Equality](#conditionals)
  1. [Blocks](#blocks)
  1. [Comments](#comments)
  1. [Whitespace](#whitespace)
  1. [Leading Commas](#leading-commas)
  1. [Semicolons](#semicolons)
  1. [Type Coercion](#type-coercion)
  1. [Naming Conventions](#naming-conventions)
  1. [Accessors](#accessors)
  1. [Constructors](#constructors)
  1. [Modules](#modules)
  1. [jQuery](#jquery)
  1. [ES5 Compatability](#es5)
  1. [Testing](#testing)
  1. [Performance](#performance)
  1. [Resources](#resources)
  1. [The JavaScript Style Guide Guide](#guide-guide)
  1. [Contributors](#contributors)
  1. [License](#license)

## <a name='tipos'>Tipos</a>

  - **Primitivos**: Quando você acessa um tipo primitivo você trabaha diretamente no seu valor

    + `String`
    + `Number`
    + `Boolean`
    + `null`
    + `undefined`

    ```javascript
    var teste = 1,
        demo = teste;

    demo = 9;

    console.log(teste, demo); // exibe: 1, 9
    ```
  - **Complex**: Quando você acessa um tipo complexo você utiliza uma referência para seu valor

    + `Object`
    + `Array`
    + `Function`

    ```javascript
    var teste = [1, 2],
        demo = teste;

    demo[0] = 9;

    console.log(teste[0], demo[0]); // exibe: 9, 9
    ```
    
    Os dois, teste[0] e demo[0], são iguais à 9 porque ao igualá-los, fazem referencia ao mesmo objeto, então alterar um é a mesma coisa que alterar o outro.
    
    Outro exemplo:
    
    ```javascript
    var pessoa1 = {nome: "João", idade: 32};
    var pessoa2 = pessoa1;

    console.log( pessoa1 ); // exibe: Object {nome: "João", idade: 32}
    console.log( pessoa2 ); // exibe: Object {nome: "João", idade: 32}

    pessoa2.nome = 'Maria';
    console.log( pessoa1 ); // exibe: Object {nome: "Maria", idade: 32}
    console.log( pessoa2 ); // exibe: Object {nome: "Maria", idade: 32}
    ```

    **[[⬆]](#TOC)**

## <a name='objetos'>Objetos</a>

  - Utilize a sintaxe literal para criação de objetos

    ```javascript
    // ruim
    var item = new Object();

    // bom
    var item = {};
    ```

  - Não utilize [palavras reservadas do javascript](https://developer.mozilla.org/en-US/docs/JavaScript/Reference/Reserved_Words) como índices de objetos.


    ```javascript
    // ruim
    var superHomem = {
      class   : 'super heroi', // class é uma palavra reservada para um possível uso futuro do javascript
      default : { clark: kent }, // default é uma palavra utilizada no 'switch ... case'
      private : true // private é uma palavra reservada para um possível uso futuro do javascript
    };

    // bom
    var superHomem = {
      classe   : 'super heroi', // escreva em portugues ou trocando uma letra (ex.: klass)
      defaults : { clark: kent }, // plural
      hidden   : true // troque por uma palavra com mesmo significado
    };
    ```
    
    Segue a lista de palavars reservadas do javascript:

    ```javascript
    // Palavras que já existem no JS 
    case        finally      switch   true       do
    catch       for          this     false      new
    continue    function     throw    while      void
    debugger    if           try      with       else
    default     in           typeof   null       return
    delete      instanceof   var        

    // Reservadas para uso futuro
    class       enum         export      const
    extends     import       super       yield
    implements  interface    let         static
    package     private      protected   public                                  
    ```

    Utilizando ```"use strict"``` o javascript avisa quando você usa alguma dessas palavras no seu código.

    Uma maneira de utilizá-las, é como string, mas é boa prática evitar se for possível. Exemplo:

  ```javascript
    // ruim
    var superHomem = {
      'class'  : 'super heroi',
      'default': { clark: kent },
      'private': true 
    };
    ```

    **[[⬆]](#TOC)**

## <a name='arrays'>Arrays</a>

  - Use a sintaxe literal para a criação de Arrays.

    ```javascript
    // ruim
    var items = new Array();

    // bom
    var items = [];
    ```

  - Por [questões de performance](http://jsperf.com/array-direct-assignment-vs-push/5) prefira declaração direta de novo valor em vez de utilizar Array#push

    ```javascript
    var tamanhoArray = items.length,
        itemsCopia = [],
        i;

    // ruim
    for (i = 0; i < tamanhoArray; i++) {
      itemsCopia.push(items[i])
    }

    // bom
    for (i = 0; i < tamanhoArray; i++) {
      itemsCopia[i] = items[i];
    }
    ```

    **[[⬆]](#TOC)**


## <a name='strings'>Strings</a>

  - Utilize aspas simples `''` para strings

    ```javascript
    // ruim
    var nome = "João Silva";

    // bom
    var nome = 'João Silva';

    // ruim
    var nomeCompleto = "João" + this.sobrenome;

    // bom
    var nomeCompleto = 'João' + this.sobrenome;
    ```

  - Strings longer than 80 characters should be written across multiple lines using string concatenation.
  - Strings maiores que 80 caracteres devem ser escritas em múltiplas linhas, usando concatenação.

    ```javascript
    // ruim
    var mensagemErro = 'Esta é uma mensagem de erro muito longa. Ela tem mais de 80 caracteres e é dificíl de ler, aumentando a chance de ter algum erro nela. Seria melhor divir ela em mais linhas.';

    // escapar quebras de linha também é ruim
    // porque dificulta encontrar erros e prejudica a compressão do código
    var mensagemErro = 'Esta é uma mensagem de erro muito longa.\
     Ela tem mais de 80 caracteres e é dificíl de ler, aumentando \
     a chance de ter algum erro nela. \
     Seria melhor divir ela em mais linhas.';

    // bom
    var mensagemErro = 'Esta é uma mensagem de erro muito longa.' +
     'Ela tem mais de 80 caracteres e é dificíl de ler, ' +
     'aumentando a chance de ter algum erro nela. ' +
     'Seria melhor divir ela em mais linhas.';
    ```

  - Quando estiver construindo uma string, prefira Array#join em vez de concatenar strings. Principalmente por causa do IE: [jsPerf](http://jsperf.com/string-vs-array-concat/2).

    ```javascript
    var items,
        messagens,
        tamanho, i;

    messagens = [{
        estado : 'successo',
        texto  : 'Este funcionou.'
    },{
        estado : 'successo',
        texto  : 'Este também funcionou'
    },{
        estado : 'erro',
        texto  : 'Mas este não funcionou.'
    }];

    tamanho = messagens.length;

    // ruim
    function inbox(messagens) {
      items = '<ul>';

      for (i = 0; i < length; i++) {
        items += '<li>' + messagens[i].texto + '</li>';
      }

      return items + '</ul>';
    }

    // bom
    function inbox(messagens) {
      items = [];

      for (i = 0; i < length; i++) {
        items[i] = '<li>' + messagens[i].texto + '</li>';
      }

      return '<ul>' + items.join('') + '</ul>';
    }
    ```

    **[[⬆]](#TOC)**


## <a name='funcoes'>Funções</a>

  - Expressões de funções:

    ```javascript
    // expressão de função anônima
    var anonima = function() {
      return true;
    };

    // expressão de função nomeada
    var funcaoNomeada = function funcaoNomeada() {
      return true;
    };

    // expressão de função auto-executável
    (function() {
      console.log('Welcome to the Internet. Please follow me.');
    })();
    ```

  - Nunca declare uma função em bloco que não seja de função. Melhor atribuir a função para uma variavel. Os Navegadores irão permitir, mas a interpretação disso é problemática, podendo gerar erros em algum momento.

    ```javascript
    // ruim
    if (usuarioAtual) {
      function teste() {
        console.log('Nooon.');
      }
    }

    // bom
    if (usuarioAtual) {
      var teste = function teste() {
        console.log('Isso!');
      };
    }
    ```

  - Never name a parameter `arguments`, this will take precendence over the `arguments` object that is given to every function scope.
  - Nunca nomeie um parâmetro como 'arguments'. Isso sobrescrevá o objeto 'arguments' que é passado para cada função.
    ```javascript
    // ruim
    function errado(nome, opcoes, arguments) {
      console.log( nome, opcoes, arguments )
    }

    // bom
    function certo(nome, opcoes, args) {
      console.log( nome, opcoes, args )
    }
    ```

    **[[⬆]](#TOC)**



## <a name='propriedades'>Propriedades</a>

  - Use ponto ```.``` para acessar propriedades.

    ```javascript
    var luke = {
      jedi  : true,
      idade : 28
    };

    // ruim
    var mestreJedi = luke['jedi'];

    // bom
    var mestreJedi = luke.jedi;
    ```

  - Use colchetes ```[]``` para acessar propriedades através de uma variável.

    ```javascript
    var luke = {
      jedi  : true,
      idade : 28
    };

    function retornaPropriedade(propriedade) {
      return luke[propriedade];
    }

    var mestreJedi = retornaPropriedade('jedi');
    ```

    **[[⬆]](#TOC)**

## <a name='variaveis'>Variáveis</a>

  - Sempre use ```var``` para declarar variáveis. Não fazer isso irá resultar em variáveis globais. Devemos evitar poluir o namespace global.


    ```javascript
    // ruim
    habilidade = new SuperPoder();

    // bom
    var habilidade = new SuperPoder();
    ```

    - Use somente uma declaração ```var``` para múltiplas variáveis e declares cada variável em uma nova linha, mantendo a identação.

    ```javascript
    // ruim
    var items = buscaItems();
    var possuiTime = true;
    var dragonball = 'z';

    // bom
    var items = buscaItems(),
        possuiTime = true,
        dragonball = 'z';
    ```

  - Declare as variáveis não inicializadas por último. Será útil depois quando você precisar declarar o valor dependendo do valor de outra já declarada.
    ```javascript
    // ruim
    var i, tamanho, dragonball,
        items = buscaItems(),
        possuiTime = true;

    // ruim
    var i, items = buscaItems(),
        dragonball,
        possuiTime = true,
        tamanho;

    // bom
    var items = buscaItems(),
        possuiTime = true,
        dragonball,
        i, tamanho;
    ```

  - Declare as variáveis no topo do escopo onde ela se encontra. Isso ajuda a evitar problemas com declaração de variáveis e [Hoisting](#hoisting).
    ```javascript
    // ruim
    function() {
      teste();
      console.log('fazendo algo..');

      //..mais código..

      var nome = buscaNome();

      if (nome === 'teste') {
        return false;
      }

      return nome;
    }

    // bom
    function() {
      var nome = buscaNome();

      teste();
      console.log('fazendo algo..');

      //..mais código..

      if (nome === 'teste') {
        return false;
      }

      return nome;
    }

    // ruim
    function() {
      var nome = buscaNome();

      if (!arguments.length) {
        return false;
      }

      return true;
    }

    // bom
    function() {
      if (!arguments.length) {
        return false;
      }

      var nome = buscaNome();

      return true;
    }
    ```

    **[[⬆]](#TOC)**


## <a name='hoisting'>Hoisting</a>

  - Variable declarations get hoisted to the top of their scope, their assignment does not.

    ```javascript
    // we know this wouldn't work (assuming there
    // is no notDefined global variable)
    function example() {
      console.log(notDefined); // => throws a ReferenceError
    }

    // creating a variable declaration after you
    // reference the variable will work due to
    // variable hoisting. Note: the assignment
    // value of `true` is not hoisted.
    function example() {
      console.log(declaredButNotAssigned); // => undefined
      var declaredButNotAssigned = true;
    }

    // The interpretor is hoisting the variable
    // declaration to the top of the scope.
    // Which means our example could be rewritten as:
    function example() {
      var declaredButNotAssigned;
      console.log(declaredButNotAssigned); // => undefined
      declaredButNotAssigned = true;
    }
    ```

  - Anonymous function expression hoist their variable name, but not the function assignment.

    ```javascript
    function example() {
      console.log(anonymous); // => undefined

      anonymous(); // => TypeError anonymous is not a function

      var anonymous = function() {
        console.log('anonymous function expression');
      };
    }
    ```

  - Named function expressions hoist the variable name, not the function name or the function body.

    ```javascript
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      superPower(); // => ReferenceError superPower is not defined

      var named = function superPower() {
        console.log('Flying');
      };


      // the same is true when the function name
      // is the same as the variable name.
      function example() {
        console.log(named); // => undefined

        named(); // => TypeError named is not a function

        var named = function named() {
          console.log('named');
        };
      }
    }
    ```

  - Function declarations hoist their name and the function body.

    ```javascript
    function example() {
      superPower(); // => Flying

      function superPower() {
        console.log('Flying');
      }
    }
    ```

  - For more information refer to [JavaScript Scoping & Hoisting](http://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting) by [Ben Cherry](http://www.adequatelygood.com/)

    **[[⬆]](#TOC)**



## <a name='conditionals'>Conditional Expressions & Equality</a>

  - Use `===` and `!==` over `==` and `!=`.
  - Conditional expressions are evaluated using coercion with the `ToBoolean` method and always follow these simple rules:

    + **Objects** evaluate to **true**
    + **Undefined** evaluates to **false**
    + **Null** evaluates to **false**
    + **Booleans** evaluate to **the value of the boolean**
    + **Numbers** evalute to **false** if **+0, -0, or NaN**, otherwise **true**
    + **Strings** evaluate to **false** if an empty string `''`, otherwise **true**

    ```javascript
    if ([0]) {
      // true
      // An array is an object, objects evaluate to true
    }
    ```

  - Use shortcuts.

    ```javascript
    // ruim
    if (name !== '') {
      // ...stuff...
    }

    // bom
    if (name) {
      // ...stuff...
    }

    // ruim
    if (collection.length > 0) {
      // ...stuff...
    }

    // bom
    if (collection.length) {
      // ...stuff...
    }
    ```

  - For more information see [Truth Equality and JavaScript](http://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108) by Angus Croll

    **[[⬆]](#TOC)**


## <a name='blocks'>Blocks</a>

  - Use braces with all multi-line blocks.

    ```javascript
    // ruim
    if (test)
      return false;

    // bom
    if (test) return false;

    // bom
    if (test) {
      return false;
    }

    // ruim
    function() { return false; }

    // bom
    function() {
      return false;
    }
    ```

    **[[⬆]](#TOC)**


## <a name='comments'>Comments</a>

  - Use `/** ... */` for multiline comments. Include a description, specify types and values for all parameters and return values.

    ```javascript
    // ruim
    // make() returns a new element
    // based on the pased in tag name
    //
    // @param <String> tag
    // @return <Element> element
    function make(tag) {

      // ...stuff...

      return element;
    }

    // bom
    /**
     * make() returns a new element
     * based on the pased in tag name
     *
     * @param <String> tag
     * @return <Element> element
     */
    function make(tag) {

      // ...stuff...

      return element;
    }
    ```

  - Use `//` for single line comments. Place single line comments on a newline above the subject of the comment. Put an emptyline before the comment.

    ```javascript
    // ruim
    var active = true;  // is current tab

    // bom
    // is current tab
    var active = true;

    // ruim
    function getType() {
      console.log('fetching type...');
      // set the default type to 'no type'
      var type = this._type || 'no type';

      return type;
    }

    // bom
    function getType() {
      console.log('fetching type...');

      // set the default type to 'no type'
      var type = this._type || 'no type';

      return type;
    }
    ```

    **[[⬆]](#TOC)**


## <a name='whitespace'>Whitespace</a>

  - Use soft tabs set to 2 spaces

    ```javascript
    // ruim
    function() {
    ∙∙∙∙var name;
    }

    // ruim
    function() {
    ∙ var name;
    }

    // bom
    function() {
    ∙∙var name;
    }
    ```
  - Place 1 space before the leading brace.

    ```javascript
    // ruim
    function test(){
      console.log('test');
    }

    // bom
    function test() {
      console.log('test');
    }

    // ruim
    dog.set('attr',{
      age: '1 year',
      breed: 'Bernese Mountain Dog'
    });

    // bom
    dog.set('attr', {
      age: '1 year',
      breed: 'Bernese Mountain Dog'
    });
    ```
  - Place an empty newline at the end of the file.

    ```javascript
    // ruim
    (function(global) {
      // ...stuff...
    })(this);
    ```

    ```javascript
    // bom
    (function(global) {
      // ...stuff...
    })(this);

    ```

    **[[⬆]](#TOC)**

  - Use indentation when making long method chains.

  ```javascript
  // ruim
  $('#items').find('.selected').highlight().end().find('.open').updateCount();

  // bom
  $('#items')
    .find('.selected')
      .highlight()
      .end()
    .find('.open')
      .updateCount();

  // ruim
  var leds = stage.selectAll('.led').data(data).enter().append("svg:svg").class('led', true)
      .attr('width',  (radius + margin) * 2).append("svg:g")
      .attr("transform", "translate(" + (radius + margin) + "," + (radius + margin) + ")")
      .call(tron.led);

  // bom
  var leds = stage.selectAll('.led')
      .data(data)
    .enter().append("svg:svg")
      .class('led', true)
      .attr('width',  (radius + margin) * 2)
    .append("svg:g")
      .attr("transform", "translate(" + (radius + margin) + "," + (radius + margin) + ")")
      .call(tron.led);
  ```

## <a name='leading-commas'>Leading Commas</a>

  - **Nope.**

    ```javascript
    // ruim
    var once
      , upon
      , aTime;

    // bom
    var once,
        upon,
        aTime;

    // ruim
    var hero = {
        firstName: 'Bob'
      , lastName: 'Parr'
      , heroName: 'Mr. Incredible'
      , superPower: 'strength'
    };

    // bom
    var hero = {
      firstName: 'Bob',
      lastName: 'Parr',
      heroName: 'Mr. Incredible',
      superPower: 'strength'
    };
    ```

    **[[⬆]](#TOC)**


## <a name='semicolons'>Semicolons</a>

  - **Yup.**

    ```javascript
    // ruim
    (function() {
      var name = 'Skywalker'
      return name
    })()

    // bom
    (function() {
      var name = 'Skywalker';
      return name;
    })();

    // bom
    ;(function() {
      var name = 'Skywalker';
      return name;
    })();
    ```

    **[[⬆]](#TOC)**


## <a name='type-coercion'>Type Coercion</a>

  - Perform type coercion at the beginning of the statement.
  - Strings:

    ```javascript
    //  => this.reviewScore = 9;

    // ruim
    var totalScore = this.reviewScore + '';

    // bom
    var totalScore = '' + this.reviewScore;

    // ruim
    var totalScore = '' + this.reviewScore + ' total score';

    // bom
    var totalScore = this.reviewScore + ' total score';
    ```

  - Numbers:

    ```javascript
    var inputValue = '4';

    // ruim
    var val = new Number(inputValue);

    // bom
    var val = Number(inputValue);

    // bom
    var val = +inputValue;
    ```

  - Booleans:

    ```javascript
    var age = 0;

    // ruim
    var hasAge = new Boolean(age);

    // bom
    var hasAge = Boolean(age);

    // bom
    var hasAge = !!age;
    ```

    **[[⬆]](#TOC)**


## <a name='naming-conventions'>Naming Conventions</a>

  - Avoid single letter names. Be descriptive with your naming.

    ```javascript
    // ruim
    function q() {
      // ...stuff...
    }

    // bom
    function query() {
      // ..stuff..
    }
    ```

  - Use camelCase when naming objects, functions, and instances

    ```javascript
    // ruim
    var OBJEcttsssss = {};
    var this_is_my_object = {};
    var this-is-my-object = {};
    function c() {};
    var u = new user({
      name: 'João Silva'
    });

    // bom
    var thisIsMyObject = {};
    function thisIsMyFunction() {};
    var user = new User({
      name: 'João Silva'
    });
    ```

  - Use PascalCase when naming constructors or classes

    ```javascript
    // ruim
    function user(options) {
      this.name = options.name;
    }

    var bad = new user({
      name: 'nope'
    });

    // bom
    function User(options) {
      this.name = options.name;
    }

    var good = new User({
      name: 'yup'
    });
    ```

  - Use a leading underscore `_` when naming private properties

    ```javascript
    // ruim
    this.__firstName__ = 'Panda';
    this.firstName_ = 'Panda';
    
    // bom
    this._firstName = 'Panda';
    ```

  - Name your functions. This is helpful for stack traces.

    ```javascript
    // ruim
    var log = function(msg) {
      console.log(msg);
    };

    // bom
    var log = function log(msg) {
      console.log(msg);
    };
    ```

    **[[⬆]](#TOC)**


## <a name='accessors'>Accessors</a>

  - Accessor functions for properties are not required
  - If you do make accessor functions use getVal() and setVal('hello')

    ```javascript
    // ruim
    dragon.age();

    // bom
    dragon.getAge();

    // ruim
    dragon.age(25);

    // bom
    dragon.setAge(25);
    ```

  - If the property is a boolean, use isVal() or hasVal()

    ```javascript
    // ruim
    if (!dragon.age()) {
      return false;
    }

    // bom
    if (!dragon.hasAge()) {
      return false;
    }
    ```

  - It's okay to create get() and set() functions, but be consistent.

    ```javascript
    function Jedi(options) {
      options || (options = {});
      var lightsaber = options.lightsaber || 'blue';
      this.set('lightsaber', lightsaber);
    }

    Jedi.prototype.set = function(key, val) {
      this[key] = val;
    };

    Jedi.prototype.get = function(key) {
      return this[key];
    };
    ```

    **[[⬆]](#TOC)**


## <a name='constructors'>Constructors</a>

  - Assign methods to the prototype object, instead of overwriting the prototype with a new object. Overwriting the prototype makes inheritance impossible: by resetting the prototype you'll overwrite the base!

    ```javascript
    function Jedi() {
      console.log('new jedi');
    }

    // ruim
    Jedi.prototype = {
      fight: function fight() {
        console.log('fighting');
      },

      block: function block() {
        console.log('blocking');
      }
    };

    // bom
    Jedi.prototype.fight = function fight() {
      console.log('fighting');
    };

    Jedi.prototype.block = function block() {
      console.log('blocking');
    };
    ```

  - Constructor methods should try to return `this`. This helps with method chaining which is often useful.

    ```javascript
    // ruim
    Jedi.prototype.jump = function() {
      this.jumping = true;
      return true;
    };

    Jedi.prototype.setHeight = function(height) {
      this.height = height;
    };

    var luke = new Jedi();
    luke.jump(); // => true
    luke.setHeight(20) // => undefined

    // bom
    Jedi.prototype.jump = function() {
      this.jumping = true;
      return this;
    };

    Jedi.prototype.setHeight = function(height) {
      this.height = height;
      return this;
    };

    var luke = new Jedi();

    luke.jump()
      .setHeight(20);
    ```


  - It's okay to write a custom toString() method, just make sure it works successfully and causes no side effects.

    ```javascript
    function Jedi(options) {
      options || (options = {});
      this.name = options.name || 'no name';
    }

    Jedi.prototype.getName = function getName() {
      return this.name;
    };

    Jedi.prototype.toString = function toString() {
      return 'Jedi - ' + this.getName();
    };
    ```

    **[[⬆]](#TOC)**


## <a name='modules'>Modules</a>

  - The module should start with a `!`. This ensures that if a malformed module forgets to include a final semicolon there aren't errors in production when the scripts get concatenated.
  - The file should be named with camelCase, live in a folder with the same name, and match the name of the single export.
  - Add a method called noConflict() that sets the exported module to the previous version.
  - Always declare `'use strict';` at the top of the module.

    ```javascript
    // fancyInput/fancyInput.js

    !function(global) {
      'use strict';

      var previousFancyInput = global.FancyInput;

      function FancyInput(options) {
        options || (options = {});
      }

      FancyInput.noConflict = function noConflict() {
        global.FancyInput = previousFancyInput;
      };

      global.FancyInput = FancyInput;
    }(this);
    ```

    **[[⬆]](#TOC)**


## <a name='jquery'>jQuery</a>

  - Prefix jQuery object variables with a `$`.

    ```javascript
    // ruim
    var sidebar = $('.sidebar');

    // bom
    var $sidebar = $('.sidebar');
    ```

  - Cache jQuery lookups.

    ```javascript
    // ruim
    function setSidebar() {
      $('.sidebar').hide();

      // ...stuff...

      $('.sidebar').css({
        'background-color': 'pink'
      });
    }

    // bom
    function setSidebar() {
      var $sidebar = $('.sidebar');
      $sidebar.hide();

      // ...stuff...

      $sidebar.css({
        'background-color': 'pink'
      });
    }
    ```

  - Scope jQuery object queries with find. [jsPerf](http://jsperf.com/jquery-find-vs-context-sel/13)

    ```javascript
    // ruim
    $('.sidebar ul').hide();

    // ruim
    $('.sidebar > ul').hide();

    // ruim
    $('.sidebar', 'ul').hide();

    // bom
    $('.sidebar').find('ul').hide();
    ```

    **[[⬆]](#TOC)**


## <a name='es5'>ECMAScript 5 Compatability</a>

  - Refer to [Kangax](https://twitter.com/kangax/)'s ES5 [compatibility table](http://kangax.github.com/es5-compat-table/)

  **[[⬆]](#TOC)**


## <a name='testing'>Testing</a>

  - **Yup.**

    ```javascript
    function() {
      return true;
    }
    ```

    **[[⬆]](#TOC)**


## <a name='performance'>Performance</a>

  - [String vs Array Concat](http://jsperf.com/string-vs-array-concat/2)
  - [Try/Catch Cost In a Loop](http://jsperf.com/try-catch-in-loop-cost)
  - [Bang Function](http://jsperf.com/bang-function)
  - [jQuery Find vs Context, Selector](http://jsperf.com/jquery-find-vs-context-sel/13)
  - Loading...

  **[[⬆]](#TOC)**


## <a name='resources'>Resources</a>


**Read This**

  - [Annotated ECMAScript 5.1](http://es5.github.com/)

**Other Styleguides**

  - [Google JavaScript Style Guide](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml)
  - [jQuery Core Style Guidelines](http://docs.jquery.com/JQuery_Core_Style_Guidelines)
  - [Principles of Writing Consistent, Idiomatic JavaScript](https://github.com/rwldrn/idiomatic.js/)

**Books**

  - [JavaScript: The Good Parts](http://www.amazon.com/JavaScript-Good-Parts-Douglas-Crockford/dp/0596517742) - Douglas Crockford
  - [JavaScript Patterns](http://www.amazon.com/JavaScript-Patterns-Stoyan-Stefanov/dp/0596806752) - Stoyan Stefanov
  - [Pro JavaScript Design Patterns](http://www.amazon.com/JavaScript-Design-Patterns-Recipes-Problem-Solution/dp/159059908X)  - Ross Harmes and Dustin Diaz
  - [High Performance Web Sites: Essential Knowledge for Front-End Engineers](http://www.amazon.com/High-Performance-Web-Sites-Essential/dp/0596529309) - Steve Souders
  - [Maintainable JavaScript](http://www.amazon.com/Maintainable-JavaScript-Nicholas-C-Zakas/dp/1449327680) - Nicholas C. Zakas
  - [JavaScript Web Applications](http://www.amazon.com/JavaScript-Web-Applications-Alex-MacCaw/dp/144930351X) - Alex MacCaw
  - [Pro JavaScript Techniques](http://www.amazon.com/Pro-JavaScript-Techniques-John-Resig/dp/1590597273) - John Resig
  - [Smashing Node.js: JavaScript Everywhere](http://www.amazon.com/Smashing-Node-js-JavaScript-Everywhere-Magazine/dp/1119962595) - Guillermo Rauch

**Blogs**

  - [DailyJS](//dailyjs.com)
  - [JavaScript Weekly](http://javascriptweekly.com/)
  - [JavaScript, JavaScript...](http://javascriptweblog.wordpress.com/)
  - [Bocoup Weblog](http://weblog.bocoup.com/)
  - [Adequately Good](http://www.adequatelygood.com/)
  - [NCZOnline](http://www.nczonline.net/)
  - [Perfection Kills](http://perfectionkills.com/)
  - [Ben Alman](http://benalman.com/)
  - [Dmitry Baranovskiy](http://dmitry.baranovskiy.com/)
  - [Dustin Diaz](http://dustindiaz.com/)
  - [net.tutsplus](http://net.tutsplus.com/?s=javascript)

  **[[⬆]](#TOC)**

## <a name='guide-guide'>The JavaScript Style Guide Guide</a>

  - [Reference](//github.com/airbnb/javascript/wiki/The-JavaScript-Style-Guide-Guide)

## <a name='authors'>Contributors</a>

  - [View Contributors](https://github.com/airbnb/javascript/graphs/contributors)


## <a name='license'>License</a>

(The MIT License)

Copyright (c) 2012 Airbnb

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

**[[⬆]](#TOC)**

# };

