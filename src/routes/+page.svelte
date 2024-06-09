<script lang="ts">
	import { data, type Data } from './data';
	let mathGrade: number[][] = [
		[30, 714, 800, 879],
		[35, 760, 850, 932],
		[40, 820, 900, 978],
		[45, 950, 970, 986]
	];

	let arr: Data[] = data;

	function calcularNota(materia: Data) {
		return (materia.nota * materia.peso).toFixed(2);
	}

	function calcularScore(arr: Data[]) {
		return arr.reduce((total, subject) => {
			return (total + subject.peso * subject.nota);
		}, 0);
	}

	function calcularSomaDosPesos(arr: Data[]) {
		return arr.reduce((sum, subject) => {
			return Math.round(sum + subject.peso);
		}, 0);
	}

	function calcularScoreFinal(arr: Data[]) {
		return (calcularScore(arr) / calcularSomaDosPesos(arr));
	}

	function calcularSimulation(arr: Data[], math: number) {
		const simuArr = JSON.parse(JSON.stringify(arr)).map((n: Data) =>
			n.prova === 'Matemática e suas tecnologias' ? { ...n, nota: math } : n
		);

		return calcularScoreFinal(simuArr);
	}
</script>
<body class="text-tertiary-500 text-xl flex-col space-between" data-theme="gold-nouveau">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<div class="bg-surface-100-800-token py-5 sm:max-lg:h-12 w-screen fixed-top"><h1 class="text-2xl px-4 font-serif"><strong>Calculadora do ENEM</strong></h1></div>
<p class="py-3 px-4 mb-12">
	Esta calculadora serve para você entender que se tivesse aprendido matemática não teria tirado
	essa nota paia 💩
</p>
<div id="tabelas" class="pl-7 mb-12 inline-block">
<table class="border-2 border-surface-700 border-collapse sm:max-md:w-screen m-auto relative">
	<tr class="bg-primary-800">
		<th>Prova do Enem</th><th>Nota mínima</th><th>Sua nota</th><th>Peso</th><th>Nota com peso</th>
	</tr>
	{#each arr as materia}
		<tr class="sm:max-xl:table-fixed text-center">
			<td class="">{materia.prova}</td>
			<td class=""><input type="number" class="rounded-lg pt-3 flex-col bg-surface-800 m-1 sm:max-lg:h-16" placeholder="0.01" bind:value={materia.minGrade} /></td>
			<td class=""><input type="number" class="rounded-lg pt-3 flex-col m-1 bg-surface-800 sm:max-lg:h-16" placeholder="780.00" bind:value={materia.nota} /></td>
			<td class=""><input type="number" class="rounded-lg pt-3 flex-col m-1 bg-surface-800 sm:max-lg:h-16" placeholder="1.50" bind:value={materia.peso} /></td>
			<td>{calcularNota(materia)}</td>
		</tr>
	{/each}

	<tr class="text-primary-700">
		<td /><td /><td>Total</td><td>Total</td>
	</tr>
	<tr>
		<td /><td /><td>{calcularScore(arr).toFixed(2)}(A)</td><td>{calcularSomaDosPesos(arr).toFixed(2)}(B)</td>
	</tr>
	<td><i>Nota do estudante (B/A) = {calcularScoreFinal(arr).toFixed(2)}</i></td>
</table>
</div>
<div id="resultado" class="variant-ghost-surface sm:max-xl:mt-72 mr-0 pt-3 text-center text-tertiary-300 rounded-xl mt-12 h-screen text-2xl sm:max-lg:text-4xl fixed-bottom">
<h3>Com base nos dados entre 2020 e 2023</h3>
{#each mathGrade as m}
	<p>Caso você tivesse acertado {m[0]} questões em matemática sua nota seria:</p>
	Entre {calcularSimulation(arr, m[1]).toFixed(2)} e {calcularSimulation(arr, m[3]).toFixed(2)}
	<br>
	com média de {calcularSimulation(arr, m[2]).toFixed(2)}

{/each}
</div>
</body>
