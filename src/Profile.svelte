<script>
    import { onMount } from "svelte";

    export let user;
    const match = user.match("/(.*)#(.*)");
    let [name, tag] = [match[1], match[2]];

    let profileData;

    onMount(() => {
        fetch(`https://valorant-playtime.ajzbc.workers.dev${escape(user)}`)
            .then((response) => response.json())
            .then((data) => {
                profileData = data;
            });
    });

    function hour(ms) {
        return (ms / 3600000).toFixed(1);
    }
</script>

<main>
    <div class="bar">
        {#if !profileData}
            <div class="icon">
                <svg
                    aria-hidden="true"
                    focusable="false"
                    data-prefix="fas"
                    data-icon="circle-notch"
                    class="spin"
                    role="img"
                    xmlns="http://www.w3.org/2000/svg"
                    viewBox="0 0 512 512"
                    ><path
                        fill="currentColor"
                        d="M288 39.056v16.659c0 10.804 7.281 20.159 17.686 23.066C383.204 100.434 440 171.518 440 256c0 101.689-82.295 184-184 184-101.689 0-184-82.295-184-184 0-84.47 56.786-155.564 134.312-177.219C216.719 75.874 224 66.517 224 55.712V39.064c0-15.709-14.834-27.153-30.046-23.234C86.603 43.482 7.394 141.206 8.003 257.332c.72 137.052 111.477 246.956 248.531 246.667C393.255 503.711 504 392.788 504 256c0-115.633-79.14-212.779-186.211-240.236C302.678 11.889 288 23.456 288 39.056z"
                    />
                </svg>
            </div>
            <p>loading...</p>
        {:else if profileData.error}
            <div class="icon">
                <svg
                    aria-hidden="true"
                    focusable="false"
                    data-prefix="fas"
                    data-icon="exclamation-triangle"
                    role="img"
                    xmlns="http://www.w3.org/2000/svg"
                    viewBox="0 0 576 512"
                    ><path
                        fill="currentColor"
                        d="M569.517 440.013C587.975 472.007 564.806 512 527.94 512H48.054c-36.937 0-59.999-40.055-41.577-71.987L246.423 23.985c18.467-32.009 64.72-31.951 83.154 0l239.94 416.028zM288 354c-25.405 0-46 20.595-46 46s20.595 46 46 46 46-20.595 46-46-20.595-46-46-46zm-43.673-165.346l7.418 136c.347 6.364 5.609 11.346 11.982 11.346h48.546c6.373 0 11.635-4.982 11.982-11.346l7.418-136c.375-6.874-5.098-12.654-11.982-12.654h-63.383c-6.884 0-12.356 5.78-11.981 12.654z"
                    /></svg
                >
            </div>
            <p>Error</p>
        {:else}
            <div class="icon">
                <img src={profileData.avatar} alt="profile" />
            </div>
            <p>{profileData.username}</p>
        {/if}
    </div>

    {#if profileData}
        {#if !profileData.error}
            <div class="col">
                <div>
                    <span>Total Time</span>
                    <span>{hour(profileData.totalTime)} hours</span>
                </div>
                {#each profileData.gamemodes as gamemode}
                    <div>
                        <span>{gamemode.name}</span>
                        <span>{hour(gamemode.time)} hours</span>
                    </div>
                {/each}
            </div>
        {:else}
            <div class="col">
                <p>
                    Player not found or Private <a
                        href="https://tracker.gg/valorant/profile/riot/{name}%23{tag}/overview"
                        >profile</a
                    >
                </p>
            </div>
        {/if}
    {/if}
</main>

<style lang="scss">
    .bar {
        img {
            height: 100%;
        }

        p {
            margin: 0;
            padding: 1rem;
            font-size: 24px;
        }
    }

    .col {
        display: flex;
        flex-direction: column;
        text-align: left;
        padding: 1rem;
        border-radius: 0.7rem;
        background-color: #1a2632;
        font-size: 18px;

        div {
            display: flex;
            margin-top: 1rem;

            span:last-child {
                margin-left: auto;
            }
        }

        div:first-child {
            margin-top: 0;
            padding-bottom: 1rem;
            border-bottom: 1px solid white;
            font-weight: 600;
        }
    }

    .spin {
        animation: spin 2s linear infinite;
    }

    @keyframes spin {
        0% {
            transform: rotate(0deg);
        }
        100% {
            transform: rotate(360deg);
        }
    }
</style>
