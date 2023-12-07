# Entregafinal

class EntidadeBibliografica {
    constructor(titulo, autor, anoPublicacao, codigo) {
        this.titulo = titulo;
        this.autor = autor;
        this.anoPublicacao = anoPublicacao;
        this.codigo = codigo;
        this.emprestado = false;
        this.usuarioEmprestimo = null;
    }

    emprestar(usuario) {
        if (!this.emprestado) {
            this.emprestado = true;
            this.usuarioEmprestimo = usuario;
            console.log(${this.titulo} foi emprestado para ${usuario}.);
        } else {
            console.log(${this.titulo} já está emprestado.);
        }
    }

    devolver() {
        if (this.emprestado) {
            this.emprestado = false;
            this.usuarioEmprestimo = null;
            console.log(${this.titulo} foi devolvido.);
        } else {
            console.log(${this.titulo} não está emprestado.);
        }
    }
}

class Livro extends EntidadeBibliografica {
    constructor(titulo, autor, anoPublicacao, codigo, genero) {
        super(titulo, autor, anoPublicacao, codigo);
        this.genero = genero;
    }

    informacoes() {
        console.log(Livro: ${this.titulo}, Autor: ${this.autor}, Ano: ${this.anoPublicacao}, Gênero: ${this.genero});
    }
}

class Revista extends EntidadeBibliografica {
    constructor(titulo, autor, anoPublicacao, codigo, edicao) {
        super(titulo, autor, anoPublicacao, codigo);
        this.edicao = edicao;
    }

    informacoes() {
        console.log(Revista: ${this.titulo}, Autor: ${this.autor}, Ano: ${this.anoPublicacao}, Edição: ${this.edicao});
    }
}


class Usuario {
    constructor(nome, registroAcademico, dataNascimento) {
        this.nome = nome;
        this.registroAcademico = registroAcademico;
        this.dataNascimento = dataNascimento;
    }
}

class Biblioteca {

    constructor() {
        this.acervo = [];
        this.usuarios = [];
    }

    adicionarItem(item) {
        this.acervo.push(item);
        console.log(`Item adicionado: ${item.titulo}`);
    }


    listarAcervo (){
        console.log('acervo da biblioteca');
        this.acervo.forEach((item, index) => {
            console.log(`${index + 1}. ${item.titulo}`);
        });
    }
        adicionarUsuario(usuario){
            this.usuarios.push(usuario);
            console.log('usuario adicionado a minha biblioteca ${usuario.nome}');
        }

        emprestrarItem(codigo,registroAcademico){
            let item = this.acervo.find((item) => item.codigo === codigo);

        let usuario = this.usuarios.find((usuario) => usuario.registroAcademico === registroAcademico);

        console.log(`item ${item.titulo} emprestado pelx ${usuario.nome}.`);

        }

        devolverItem(codigo){
            let item = this.acervo.find((item) => item.codigo === codigo);

            console.log(`item ${item.titulo} retornado ao acervo.`);
        }

        async carregarDadosDaAPI(url) {
            try {
                const response = await fetch(url);
                const data = await response.json();
                
                for (const itemData of data) {
                    if (itemData.tipo === "Livro") {
                        const livro = new Livro(
                            itemData.titulo,
                            itemData.autor,
                            itemData.anoPublicado,
                            itemData.codigo,
                            false,
                            null,
                            itemData.genero
                        );
                        this.adicionarItem(livro);
                    } else if (itemData.tipo === "Revista") {
                        const revista = new Revista(
                            itemData.titulo,
                            itemData.autor,
                            itemData.anoPublicado,
                            itemData.codigo,
                            false,
                            null,
                            itemData.edicao
                        );
                        this.adicionarItem(revista);
                    }
                }
    
                console.log("dados da API carregados com sucesso");
            } catch (error) {
                console.error("erro ao carregar os dados da API:", error);
            }
        }
    }   

const biblioteca = new Biblioteca();

const usuario1 = new Usuario("kety",8768,"2003-08-19");
const usuario2 = new Usuario("amanda",6954,"2005-03-13");
const usuario3= new Usuario("ana",2564,"2004-06-09");
const usuario4 = new Usuario("kauanni",1557,"2003-03-31");
const usuario5 = new Usuario("carol",2369,"2007-10-11");


minhaBiblioteca.carregarDadosDaAPI("https://api-biblioteca-mb6w.onrender.com/acervo")
    .then(() => {
        minhaBiblioteca.listarAcervo();

        minhaBiblioteca.adicionarUsuario(usuario1);

        minhaBiblioteca.emprestarItem(813, 8768);
        minhaBiblioteca.emprestarItem(587, 1557);

        minhaBiblioteca.listarAcervo();

        const livro1 = minhaBiblioteca.acervo.find((item) => item.codigo === 813);
        livro1.informacoes();

        minhaBiblioteca.devolverItem(813);

        minhaBiblioteca.listarAcervo();
    })

    .catch(error => console.error('Erro ao obter dados da API:', error));
