<template>
	<div id="app">
		<h1>Registrar Reclamação</h1>
		<div class="conteudo">
			<div class="painel" v-if="!enviado">
				<div class="cabecalho">Formulário</div>
				<Rotulo nome="E-mail">
					<!-- <input type="text" v-model="usuario.email"> -->
					<!-- modificadores de inputs, lazy, atualiza quando sai do campo-->
					<!-- <input type="text" v-model.lazy="usuario.email">  -->
					<!-- remove os espaços das extremidades -->
					<input type="text" v-model.lazy.trim="usuario.email"> 
				</Rotulo>
				<Rotulo nome="Senha">
					<input type="password" v-model="usuario.senha">
				</Rotulo>
				<Rotulo nome="Idade">
					<!-- <input type="number" v-model="usuario.idade"> -->
					<!-- transforma string pra numeros number mesmo depois das atualizações, somente em tipos numéricos-->
					<input type="number" v-model.number="usuario.idade">
				</Rotulo>
				<Rotulo nome="Mensagem">
					<textarea name="" cols="30" rows="5" v-model="mensagens"></textarea>
				</Rotulo>
				<Rotulo nome="Características do Problema">
					<span class="mr-4"><input type="checkbox" v-model="caracteristicas"
						value="reproduzivel"> Reproduzível</span>
					<span><input type="checkbox" v-model="caracteristicas"
						value="intermitente"> Intermitente</span>
				</Rotulo>
				<Rotulo nome="Qual produto?">
					<span class="mr-4"><input type="radio" value="web" v-model="produto"> Web</span>
					<span class="mr-4"><input type="radio" value="mobile" v-model="produto"> Mobile</span>
					<span><input type="radio" value="outro" v-model="produto"> Outro</span>
				</Rotulo>
				<Rotulo nome="Prioridade">
					<select v-model="prioridade">
						<!--:value="prioridade.codigo" ausente, seleciona valor -->
						<option v-for="prioridade in prioridades"
							:value="prioridade.codigo"  :key="prioridade.codigo"
							:selected="prioridade.codigo === 1"  > <!-- valor padrão -->
								{{ prioridade.codigo }} - {{ prioridade.nome }}
						</option>
					</select>
				</Rotulo>
				<Rotulo nome="Primeira Reclamação?">
					<!-- como o v-model funciona por baixo dos panos -->
					<input type="text" :value="temp" @input="temp = $event.target.value">
				</Rotulo>
				<Rotulo nome="Primeira Reclamação?">
					<Escolha v-model="escolha" />
				</Rotulo>
				<hr>
				<button @click.prevent="enviar">Enviar</button>
			</div>
			<div class="painel" v-else>
				<div class="cabecalho">Resultado</div>
				<Rotulo nome="E-mail">
					<span>{{ usuario.email }}</span>
				</Rotulo>
				<Rotulo nome="Senha">
					<span>{{ usuario.senha }}</span>
				</Rotulo>
				<Rotulo nome="Idade">
					<!-- <span>{{ usuario.idade }}</span> -->
					<span>{{ usuario.idade }} {{ tipoIdade }}</span>
				</Rotulo>
				<Rotulo nome="Mensagem">
					<!-- <span>{{ mensagens }}</span> -->
					<!-- preservar os espaços e quebra de linhas -->
					<span style="white-space: pre;">{{ mensagens }}</span>
				</Rotulo>
				<Rotulo nome="Marque as Opções">
					<span>
						<ul>
							<li v-for="c in caracteristicas" :key="c">{{ c }}</li>
						</ul>
					</span>
				</Rotulo>
				<Rotulo nome="Qual produto?">
					<span>{{ produto }}</span>
				</Rotulo>
				<Rotulo nome="Prioridade">
					<span>{{ prioridade }} - {{ tipoPrioridade }} </span>
				</Rotulo>
				<Rotulo nome="Primeira Reclamação?">
					<span>{{ temp }}</span>
				</Rotulo>
				<Rotulo nome="Primeira Reclamação?">
					<span>{{ escolha }}</span>
				</Rotulo>
			</div>
		</div>
	</div>
</template>

<script>
import Rotulo from './components/Rotulo.vue'
import Escolha from './components/Escolha.vue'

export default {
	name: 'app',
	components: { Rotulo, Escolha },
	computed: {
		tipoIdade() {
			return typeof this.usuario.idade //retorna o tipo
		},
		tipoPrioridade() {
			return typeof this.prioridade
		}
	},
	methods: {
		enviar() {
			this.enviado = true
		}
	},
	data() {
		return {
			// email: '',
			//funciona usuários, propriedades criadas pelo js dinâmicamentes	
			// usuario: {
		
			// }
			//agrupar os dados
			mensagens: '',
			caracteristicas: [], //check
			produto: 'web',
			prioridade: 1,
			prioridades: [
				{ codigo: 1, nome: 'Baixa' },
				{ codigo: 2, nome: 'Moderada' },
				{ codigo: 3, nome: 'Alta' }
			],
			usuario: {
				email: '',
				senha: '',
				idade: 25,
			},
			temp: 'teste',
			escolha: true,
			enviado: false
		}
	},

	created() {
		//two way data bind
		// setTimeout(() => {
		// 	this.email = ''
		// }, 5000);
	}
}
</script>

<style>

body {
	background-color: #ECECEC;
}

#app {
	font-family: 'Avenir', Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	text-align: center;
	color: #2c3e50;

	display: flex;
	flex-direction: column;
}

.conteudo {
	display: flex;
}

.painel {
	flex: 1;
	background: #FFF;
	margin: 0px 10px;
	padding: 20px;
	border: 1px solid #AAA;
	border-radius: 5px;
}

.painel .cabecalho {
	width: 100%;
	background-color: #DDD;
	padding: 10px 0px;
	border-radius: 5px;
	font-size: 1.4rem;
}

.painel button {
	float: right;
	margin: 10px 0px;
	padding: 10px 20px;
	font-size: 1.4rem;
	border-radius: 5px;
	color: #FFF;
	background-color: #2196F3;
}

#app h1 {
	font-weight: 200;
	margin: 20px;
	padding: 0;
}

.mr-4 {
	margin-right: 40px;
}
</style>
