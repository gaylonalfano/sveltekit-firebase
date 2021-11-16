<script context="module" lang="ts">
	import { fb, db } from '$lib/firebase/config';
	import { collection, getDocs } from 'firebase/firestore';

	export async function load({ page, fetch, session, stuff }) {
		const collectionRef = collection(db, 'transactions');
		let transactions = [];

		// NOTE Must AWAIT or doesn't always get the data!
		await getDocs(collectionRef)
			.then((snapshot) => {
				snapshot.docs.forEach((doc) => {
					transactions.push({
						...doc.data(),
						id: doc.id
					});
				});
				// NOTE Runs on the SERVER!
				console.log(transactions);
			})
			.catch((error) => {
				console.log(error.message);
				return {
					error
				};
			});

		return { props: { transactions } };

		// const collectionRef = await collection(db, 'transactions');
		// let transactions = [];
		// try {
		// 	getDocs(collectionRef).then((snapshot) => {
		// 		// console.log(snapshot.docs);
		// 		snapshot.docs.forEach((doc) => {
		// 			transactions.push({
		// 				...doc.data(),
		// 				id: doc.id
		// 			});
		// 		});
		// 		// NOTE Runs on the SERVER!
		// 		console.log(transactions);
		// 	});
		// 	return { props: { transactions } };
		// } catch (error) {
		// 	return {
		// 		status,
		// 		error
		// 	};
		// }
	}
</script>

<script lang="ts">
	// NOTE Runs in the CLIENT/BROWSER!
	export let transactions;
</script>

<svelte:head>
	<title>Home</title>
</svelte:head>

<h1>NFTYGMI</h1>
<section>
	{#each transactions as { id, transactionType, transactionCoin, nftId, nftCollection } (id)}
		<h3>{id}</h3>
		<ul>
			<li>{transactionType}</li>
			<li>{transactionCoin}</li>
			<li>{nftId}</li>
			<li>{nftCollection}</li>
		</ul>
	{/each}
</section>

<style>
	section {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		flex: 1;
	}

	h1 {
		width: 100%;
	}
</style>
