<script>
	import { onMount } from 'svelte';
	import UserList from './components/UserList.svelte';
	import Login from './components/Login.svelte';
	import Regist from './components/Regist.svelte';

	export let currentView = 'login';
	let users = [];

	async function showUserList() {
		users = [];
		for (let i = 0; i < sessionStorage.length; i++) {
			const key = sessionStorage.key(i);
			const userData = sessionStorage.getItem(key);
			if (userData) {
				users.push(JSON.parse(userData));
			}
		}
		currentView = 'users';
	}


	function showLogin() {
		currentView = 'login';
	}

	function showRegister() {
		currentView = 'register';
	}

	function onLoginSuccess() {
		showUserList();
	}

	function onRegisterSuccess() {
		showUserList();
	}
</script>

{#if currentView === 'login'}
	<Login on:loginSuccess={onLoginSuccess} showRegister={showRegister}/>
{/if}

{#if currentView === 'register'}
	<Regist on:registerSuccess={onRegisterSuccess} showLogin={showLogin}/>
{/if}

{#if currentView === 'users'}
	<UserList users={users} />
{/if}