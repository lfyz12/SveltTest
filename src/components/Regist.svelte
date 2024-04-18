<script>
    import { navigate } from 'svelte-routing';
    import bcrypt from 'bcryptjs';
    import { createEventDispatcher } from 'svelte';

    export let showLogin;
    let username = '';
    let email = '';
    let password = '';
    let confirmPassword = '';
    let message = '';

    const dispatch = createEventDispatcher();

    async function register(event) {
        event.preventDefault();

        if (password !== confirmPassword) {
            message = 'Пароли не совпадают.';
            return;
        }

        const salt = await bcrypt.genSalt(10);
        const hashedPassword = await bcrypt.hash(password, salt);

        const user = {
            username,
            email,
            password: hashedPassword
        };

        sessionStorage.setItem(email, JSON.stringify(user));

        username = '';
        email = '';
        password = '';
        confirmPassword = '';

        message = 'Пользователь успешно зарегистрирован';

        dispatch('registerSuccess');
    }
</script>

<form on:submit|preventDefault={register}>
    <input type="text" bind:value={username} placeholder="Имя пользователя" required/>
    <input type="email" bind:value={email} placeholder="Email" required/>
    <input type="password" bind:value={password} placeholder="Пароль" required/>
    <input type="password" bind:value={confirmPassword} placeholder="Подтверждение пароля" required/>
    <button type="submit">Зарегистрироваться</button>
</form>
<p>{message}</p>
<button on:click={showLogin}>Есть аккаунт? Войти</button>