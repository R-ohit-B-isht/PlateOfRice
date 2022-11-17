<script>
   import { initializeApp } from 'firebase/app';
	import {
		getAuth,
		signInWithEmailAndPassword,
		signOut,
		 Users,
		onAuthStateChanged,
		signInWithPopup,
		GoogleAuthProvider,
		GithubAuthProvider
	} from 'firebase/auth';
	import { onMount } from 'svelte';

	let email = '';
	let password = '';
	let user= Users | null;

	const firebaseConfig = {
		apiKey: "AIzaSyCN9-58VEIInSrUZZuF6rRhhp7oO3Csh8Q",
  authDomain: "plateofrice-44449.firebaseapp.com",
  projectId: "plateofrice-44449",
  storageBucket: "plateofrice-44449.appspot.com",
  messagingSenderId: "573486662259",
  appId: "1:573486662259:web:f5ac14b9ce842ca538a006",
  measurementId: "G-5KTE5QB1PY"
	};

	const app = initializeApp(firebaseConfig);

	const login = () => {
		const auth = getAuth(app);
		signInWithEmailAndPassword(auth, email, password).catch((error) => {
			const errorCode = error.code;
			const errorMessage = error.message;
			console.log(errorCode, errorMessage);
		});
	};

	const loginWithGoogle = () => {
		const auth = getAuth(app);
		signInWithPopup(auth, new GoogleAuthProvider());
	};

	const loginWithGithub = () => {
		const auth = getAuth(app);
		signInWithPopup(auth, new GithubAuthProvider());
	};

	const logout = async () => {
		const auth = getAuth(app);
		signOut(auth);
	};

	onMount(async () => {
		const auth = getAuth(app);
		onAuthStateChanged(auth, (newUser) => {
			console.log(user);
			user = newUser;
		});
	});
</script>
{#if user}
	<p>Signed in with {user.providerData[0].providerId}!</p>
	<button on:click={logout}>Logout</button>
{:else}
	<input type="email" id="email" placeholder="email" bind:value={email} />
	<input type="password" id="password" placeholder="password" bind:value={password} />
	<button on:click={login}>Login</button>
	<button on:click={loginWithGoogle}>Login with Google</button>
	<button on:click={loginWithGithub}>Login with Github</button>
{/if}