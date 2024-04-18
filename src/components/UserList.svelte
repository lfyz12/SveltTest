<script>
    export let users = [];
</script>

{#if users.length > 0}
    <table>
        <thead>
        <tr>
            <th>Имя пользователя</th>
            <th>Email</th>
        </tr>
        </thead>
        <tbody>
        {#each users as user}
            <tr>
                <td>{user.username}</td>
                <td>{user.email}</td>
            </tr>
        {/each}
        </tbody>
    </table>
{/if}