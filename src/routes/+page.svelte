<script lang='ts'>
	import { fromString, toString, xor } from 'uint8arrays';

	const cipher1 = "315c4eeaa8b5f8aaf9174145bf43e1784b8fa00dc71d885a804e5ee9fa40b16349c146fb778cdf2d3aff021dfff5b403b510d0d0455468aeb98622b137dae857553ccd8883a7bc37520e06e515d22c954eba5025b8cc57ee59418ce7dc6bc41556bdb36bbca3e8774301fbcaa3b83b220809560987815f65286764703de0f3d524400a19b159610b11ef3e";

	const cipher2 = "234c02ecbbfbafa3ed18510abd11fa724fcda2018a1a8342cf064bbde548b12b07df44ba7191d9606ef4081ffde5ad46a5069d9f7f543bedb9c861bf29c7e205132eda9382b0bc2c5c4b45f919cf3a9f1cb74151f6d551f4480c82b2cb24cc5b028aa76eb7b4ab24171ab3cdadb8356f";

	const cipher3 = "32510ba9a7b2bba9b8005d43a304b5714cc0bb0c8a34884dd91304b8ad40b62b07df44ba6e9d8a2368e51d04e0e7b207b70b9b8261112bacb6c866a232dfe257527dc29398f5f3251a0d47e503c66e935de81230b59b7afb5f41afa8d661cb";

	const cipher4 = "32510ba9aab2a8a4fd06414fb517b5605cc0aa0dc91a8908c2064ba8ad5ea06a029056f47a8ad3306ef5021eafe1ac01a81197847a5c68a1b78769a37bc8f4575432c198ccb4ef63590256e305cd3a9544ee4160ead45aef520489e7da7d835402bca670bda8eb775200b8dabbba246b130f040d8ec6447e2c767f3d30ed81ea2e4c1404e1315a1010e7229be6636aaa";

	const cipher5 = "3f561ba9adb4b6ebec54424ba317b564418fac0dd35f8c08d31a1fe9e24fe56808c213f17c81d9607cee021dafe1e001b21ade877a5e68bea88d61b93ac5ee0d562e8e9582f5ef375f0a4ae20ed86e935de81230b59b73fb4302cd95d770c65b40aaa065f2a5e33a5a0bb5dcaba43722130f042f8ec85b7c2070";

	const cipher6 = "32510bfbacfbb9befd54415da243e1695ecabd58c519cd4bd2061bbde24eb76a19d84aba34d8de287be84d07e7e9a30ee714979c7e1123a8bd9822a33ecaf512472e8e8f8db3f9635c1949e640c621854eba0d79eccf52ff111284b4cc61d11902aebc66f2b2e436434eacc0aba938220b084800c2ca4e693522643573b2c4ce35050b0cf774201f0fe52ac9f26d71b6cf61a711cc229f77ace7aa88a2f19983122b11be87a59c355d25f8e4";

	const cipher7 = "32510bfbacfbb9befd54415da243e1695ecabd58c519cd4bd90f1fa6ea5ba47b01c909ba7696cf606ef40c04afe1ac0aa8148dd066592ded9f8774b529c7ea125d298e8883f5e9305f4b44f915cb2bd05af51373fd9b4af511039fa2d96f83414aaaf261bda2e97b170fb5cce2a53e675c154c0d9681596934777e2275b381ce2e40582afe67650b13e72287ff2270abcf73bb028932836fbdecfecee0a3b894473c1bbeb6b4913a536ce4f9b13f1efff71ea313c8661dd9a4ce";

	const cipher8 = "315c4eeaa8b5f8bffd11155ea506b56041c6a00c8a08854dd21a4bbde54ce56801d943ba708b8a3574f40c00fff9e00fa1439fd0654327a3bfc860b92f89ee04132ecb9298f5fd2d5e4b45e40ecc3b9d59e9417df7c95bba410e9aa2ca24c5474da2f276baa3ac325918b2daada43d6712150441c2e04f6565517f317da9d3";

	const cipher9 = "271946f9bbb2aeadec111841a81abc300ecaa01bd8069d5cc91005e9fe4aad6e04d513e96d99de2569bc5e50eeeca709b50a8a987f4264edb6896fb537d0a716132ddc938fb0f836480e06ed0fcd6e9759f40462f9cf57f4564186a2c1778f1543efa270bda5e933421cbe88a4a52222190f471e9bd15f652b653b7071aec59a2705081ffe72651d08f822c9ed6d76e48b63ab15d0208573a7eef027";

	const cipher10 = "466d06ece998b7a2fb1d464fed2ced7641ddaa3cc31c9941cf110abbf409ed39598005b3399ccfafb61d0315fca0a314be138a9f32503bedac8067f03adbf3575c3b8edc9ba7f537530541ab0f9f3cd04ff50d66f1d559ba520e89a2cb2a83";

	const targetcipher = "32510ba9babebbbefd001547a810e67149caee11d945cd7fc81a05e9f85aac650e9052ba6a8cd8257bf14d13e6f0a803b54fde9e77472dbff89d71b57bddef121336cb85ccb8f3315f4b52e301d16e9f52f904";




	function emptyPlainText(len) {
		return Array(len).fill("_");
	}

	function tryxor(string1, string2) {
		const len = Math.max(string1.length, string2.length);

		if (string1.length > string2.length) {
			string2 = string2.padEnd(len, "0");
		} else {
			string1 = string1.padEnd(len, "0");
		}
		
		const cipher1 = fromString(string1, 'base16');
		const cipher2 = fromString(string2, 'base16');
		const plaintextxor = xor(cipher1, cipher2);

		return plaintextxor;	
	}

	const allciphers = [cipher1, cipher2, cipher3, cipher4, cipher10, cipher6, cipher7, cipher8, cipher9, targetcipher];

	const xoredArrays = [];

	const alltexts = allciphers.map((cipher, idx) => { 
		return { 
			name: idx+1,
			len: cipher.length / 2,
			ciphertext: cipher,
			plaintext: emptyPlainText(cipher.length / 2),
			spacetext: "",
			spaces: []
		}; 
	});

	// Get xored arrays
	for (let i = 0; i < alltexts.length; i++) {
		xoredArrays[i] = [];
		for (let j = 0; j < alltexts.length; j++) {
			if (i == j) {
				xoredArrays[i][j] = {
					note: "Not used",
					len: 0,
					xored: "XXX",
				}
			} else if (i > j) {
				xoredArrays[i][j] = xoredArrays[j][i];
			} else {
				xoredArrays[i].push({
					note: `${alltexts[i].name}x${alltexts[j].name}`.padStart(4, " "),
					len: alltexts[i].len,
					xored: tryxor(alltexts[i].ciphertext, alltexts[j].ciphertext)
				})
			}
		}
	}



	function isAlphaOrNull(n) {
		return (n >= 65 && n < 91) || (n >= 97 && n < 123) || n == 0;
	}

	function guessSpaces(cipherlen, xoredArrs) {
		let spaces = [];

		for (let i = 0; i < cipherlen; i++) {
			let result = 0;
			for (const current of xoredArrs) {
				if (current.len < 1) { continue; }

				if (i < current.len) {
					if (!isAlphaOrNull(current.xored[i])) {
						result = 0;
						break;
					} else {
						result += 1;
					}
				} else {
					continue;
				}
			}

			if (result > 1) {
				spaces.push(i);
			}
		}

		return spaces;
	}

	// First processing
	alltexts.forEach((item, idx) => {
		item.spaces = guessSpaces(item.len, xoredArrays[idx]);
	
		item.spaces.forEach(space => item.plaintext[space] = " ");
		item.spacetext = item.plaintext.join('').replaceAll(" ", "X");
	});

	// Start replacing
	console.log(xoredArrays);
	const steps = [];
	for (let i = 0; i < alltexts.length; i++) {
		steps[i] = [];
		for (let j = 0; j < alltexts.length; j++) {
			// if (i != j) {

				alltexts[i].spaces.forEach(pos => {
					const charnum = xoredArrays[i][j].xored[pos] ^ " ".charCodeAt(0);	// Flip with space
					if (pos < alltexts[j].plaintext.length) {
						alltexts[j].plaintext[pos] = String.fromCharCode(charnum);	
					}
				})

				steps[i].push({
					name: alltexts[j].name,
					plaintext: alltexts[j].plaintext.join('')
				})
			// }
		}
	}

	
	// Second processing
	// const steps = alltexts.map((item, idx) => {
	// 	const step = [];
	// 	for (let i = 0; i < alltexts.length; i++) {
	// 		if (idx == i) { continue; }

	// 		const replacedPlaintext = alltexts[i].plaintext;

	// 		step.push({
	// 			name: alltexts[i].name,
	// 			plaintext: item.spaces
	// 		})
			
	// 	}
	// 	item.spaces

	// 	return step;
	// })

	// function isletter(char) {
	// 	return char.match('[a-zA-Z]');
	// }
	function toReadableLetter(n) {
		if ((n >= 65 && n < 91) || (n >= 97 && n < 123)) {
			return String.fromCharCode(n);
		} else if (n == 0) {
			return " ";
		} else {
			return "_";
		}
	}

	let cribposition = $state(0);
	let chosencipher = $state(1);

	let cribword = $state("");
	let cribdisplay = $derived(cribword.padStart(cribposition + cribword.length, " "));

	function toMeaningfulChar(n) {
		if (n >= 32 && n < 127) {
			return String.fromCharCode(n);
		} else {
			return "_";
		}
	}

	function getCorrespondingCrib(i, j, position, word) {
		if (i == j) { return word; }

		const byteArray = fromString(word, 'ascii');
		const checkArray = xoredArrays[i][j].xored.slice(position, position + word.length);

		return Array.from(xor(byteArray, checkArray)).map(num => toMeaningfulChar(num)).join('');
	}

	function padSpace(n) {
		return "".padStart(n, " ");
	}

	let displaytexts = $state(alltexts.map(text => text.plaintext.join('')));

	function acceptGuess() {
		for (let i = 0; i < displaytexts.length; i++) {
			const replacement = getCorrespondingCrib(chosencipher-1, i, cribposition, cribword);
			displaytexts[i] = 
				displaytexts[i].substring(0, cribposition) + 
				replacement + 
				displaytexts[i].substring(cribposition + replacement.length);
		}
	}
</script>


<h1 class="text-3xl font-bold">Many Time Pad Solution - Dang Viet Hoang 202152026</h1>

<h3 class="my-2 text-xl font-bold">1. Preliminary</h3>

<p>
When two ciphertexts are XORed together, the result is equivalent to XORing the two corresponding plaintexts together if the same key was used in both encryption:
</p>

<pre>
<code>
	C1 = P1 ⊕ K
	C2 = P2 ⊕ K
	Then: C1 ⊕ C2 = (P1 ⊕ K) ⊕ (P2 ⊕ K) = P1 ⊕ P2

</code></pre>

<p>
Another interesting property of XOR is that XORing a space (0x20 in ASCII) with an alphabetical character toggles the case of that character. For example, XORing the lowercase letter 'a' (0x61) with space (0x20) results in the uppercase 'A' (0x41), and vice versa.
</p>

<p>
Therefore, if we know where the spaces are in one plaintext, we can use the result of XOR to infer information about another plaintext. That's why we will try to find as many spaces as possible by XORing all possible combinations of ciphertexts.
</p>

<h3 class="my-2 text-xl font-bold">2. Find all the spaces</h3>

We will start by looking at the result of the first ciphertext XORing with other ciphertexts, including the secret. Any character that isn't a letter will be denoted as an underscore "_"
<br>
Color <span class="bg-primary"> blue </span> means that XOR result is 0x00, which indicates the two plaintexts have the same character at that position.
<br>
Color <span class="bg-error"> red </span> is to denote the extra length not needed for consideration of this plaintext.
<br>
<br>

<div class="flex flex-col gap-4">

{#each alltexts as checkspace, index (checkspace.name)}
	<div class="font-bold">For the plaintext [{checkspace.name}]:</div>

	<div class="font-mono whitespace-pre">
		{#each xoredArrays[index].toSpliced(index,1) as uintarray}
			<div>{uintarray.note}:
				{#each uintarray.xored as charnum, index}
					<span class:bg-primary={charnum == 0}
						class:bg-error={index >= uintarray.len}>
						{toReadableLetter(charnum)}
					</span>
				{/each}			
			</div>
		{/each}

		<div class="font-sans">The spaces in plaintext [{checkspace.name}] will be:</div>

		<div>&nbsp;[{checkspace.name}]: {checkspace.spacetext}</div>
	</div>

	Replacing letters in other plaintexts using [{checkspace.name}] known space:

	<div class="font-mono whitespace-pre">
		<div>&nbsp;[{checkspace.name}]: {checkspace.spacetext}</div>

		{#each steps[index] as step}
			<div>{`[${step.name}]`.padStart(4, " ")}: {step.plaintext}</div>
		{/each}
	</div>
{/each}

</div>

<h3 class="my-2 text-xl font-bold">3. Guess the rest</h3>

We will then guess the rest by sliding a word and then XOR it with each texts to see which one works for ALL plaintexts.

<div class="mt-2 font-mono">

	<span>Cipher number: </span>
	<input type="number" min="1" max={alltexts.length} bind:value={chosencipher} 
		class="input input-sm input-bordered input-primary rounded-lg my-2"/>
	<span>Guess word:</span>
	<input type="text" bind:value={cribword} 
		class="input input-sm input-bordered input-primary rounded-lg" />

	<button class="btn btn-sm btn-primary" onclick={acceptGuess}>Accept guess</button>

	<input type="range" min="0" max={alltexts[7].len - cribword.length} bind:value={cribposition} 
		class={`range range-xs w-[${alltexts[7].len - cribword.length}ch]`} step="1" />


	{#each alltexts as text, index (text.name)}
		<div class="whitespace-pre">
			{`[${text.name}]`.padStart(4, " ")}:
			<input type="text" bind:value={displaytexts[index]}
				class="input h-fit px-0 rounded-lg w-full" spellcheck="false">
		</div>
		{#if chosencipher-1 == index}
			<div class="whitespace-pre text-success font-bold">XXXXX {padSpace(cribposition)}{cribword}</div>
		{:else}
			<div class="whitespace-pre">----- {padSpace(cribposition)}{getCorrespondingCrib(chosencipher-1, index, cribposition, cribword)}</div>
		{/if}
	{/each}

</div>


<div class="font-lg mt-4">
The secret is "The secret message is: When using a stream cipher, never use the key more than once"
</div>

