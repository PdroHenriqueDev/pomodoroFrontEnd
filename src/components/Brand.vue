<template>
    <div class="brand-container">
        <div>
            <IconifyIcon :icon="icons.tomatoIcon" class="icon"/>
            <div class="brand-text">Pomodoro</div>
        </div>
    </div>
    
    
</template>

<script>

import IconifyIcon from '@iconify/vue';
import tomatoIcon from '@iconify/icons-noto/tomato';

export default ({
    components: {
        IconifyIcon,
    },
    data() {
        return {
            icons: {
				tomatoIcon,
    
			},
        }
    },
})
</script>

<style scoped>
/*.brand-container{
    margin: 2rem;
}*/

.brand-container div{
    display: flex;
    flex-direction: row;
    align-items: center;
}

.brand-container div div{
    margin-left: 1rem;
}

.brand-text {
    font-family: Russo One;
    font-size: 70px;
    font-style: normal;
    font-weight: 400;
    line-height: 96px;
    color: white;
}
.icon {
    height: 80px;
    width: 100px;
    color: white;
}

</style>
