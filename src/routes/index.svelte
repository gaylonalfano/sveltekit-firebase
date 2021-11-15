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
	{#if transactions.length !== 0}
		{#each transactions as transaction (transaction.id)}
			<h3>{transaction.id}</h3>
			<ul>
				<li>{transaction.transactionType}</li>
				<li>{transaction.transactionCoin}</li>
				<li>{transaction.nftId}</li>
				<li>{transaction.nftCollection}</li>
			</ul>
		{/each}
	{/if}
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
