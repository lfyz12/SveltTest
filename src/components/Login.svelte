<script>
    import bcrypt from 'bcryptjs';
    import {createEventDispatcher} from 'svelte';

    export let showRegister;
    let email = '';
    let password = '';
    let message = '';

    const dispatch = createEventDispatcher();

    async function login(event) {
        event.preventDefault();

        const storedUser = JSON.parse(sessionStorage.getItem(email));

        if (storedUser) {
            const passwordMatch = await bcrypt.compare(password, storedUser.password);
            if (passwordMatch) {
                message = 'Авторизация успешна!';
                dispatch('loginSuccess');
            } else {
                message = 'Неверный email или пароль.';
            }
        } else {
            message = 'Пользователь не найден.';
        }
    }
</script>

<form on:submit|preventDefault={login}>
    <input type="email" bind:value={email} placeholder="Email" required/>
    <input type="password" bind:value={password} placeholder="Пароль" required/>
    <button type="submit">Войти</button>
</form>
<p>{message}</p>

<button on:click={showRegister}>Нет аккаунта? Зарегистрироваться</button>