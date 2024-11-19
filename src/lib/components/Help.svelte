<script>
    const commands = ['help', 'whoami', 'projects', 'contact', 'clear', 'main'];
</script>

&#8618;&nbsp;<b>Available commands:</b>
<div>
    <ul>
        {#each commands as command}
            <li>&nbsp;&#x2022;&nbsp;{command}</li>
        {/each}
    </ul>
</div>