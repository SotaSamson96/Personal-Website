<template>
  <div id="skills">
        <h1>Skills</h1>
        <div class="skillsBar">
            <div
                class="skillBtn"
                v-for="skill in skills"
                :id="skill + 'Btn'"
                :key="skill"
                @click="selectSkill(skill)">
                {{skill}}
            </div>
        </div>
        <div id="tagsGrid">
            <Tag 
                v-for="(category, tech) in stack"
                :class="[category]"
                :key="tech"
                :tag="tech"
            />
        </div>
  </div>
</template>

<script>
import Tag from './TechTag';
import Isotope from 'isotope-layout';
import Info from '../info.js';

export default {
    name: 'Skills',
    components: {
        Tag
    },
    data() {
        return {
            skills: ['All', 'Front-End', 'Back-End', 'Other'],
            current: 'All'
        }
    },
    computed: {
        stack: function() {
            return Info['stack'];
        }
    },
    methods: {
        selectSkill(skill) {
            const className = 'skillSelected';
            const previousBtn = document.getElementById(this.current + 'Btn');
            previousBtn.classList.remove(className);
            this.current = skill;
            const currentBtn = document.getElementById(skill + 'Btn');
            currentBtn.classList.add(className);
            this.sortTags(skill);
        },
        sortTags(skill) {
            let category = skill === 'All' ? '' : skill;
            this.iso.arrange({
                filter: (itemElem) => {
                    return itemElem.classList.value.includes(category);
                }
            })
        }
    },
    mounted() {
        const grid = document.getElementById('tagsGrid');
        const iso = new Isotope(grid, {
            itemSelector: '.tag',
            layoutMode: 'fitRows'
        });
        this.iso = iso;
        document.getElementById(this.current + 'Btn').classList.add('skillSelected');
    }
}
</script>


<style scoped lang="scss">
    @import '../assets/sass/mixins.scss';

    #skills {
        width: calc(100% - 50px);
        min-height: 100vh;
        display: flex;
        flex-direction: column;
        align-items: center;
        margin-top: 1.5rem;
        color: white;
        background-color: var(--primary-color);
        > h1 {
            width: 100%;
            border-bottom: 3px solid white;
            font: {
                size: 5em;
                weight: 200;
                style: italic;
            }
        }
    }

    .skillsBar {
        width: 100%;
        display: flex;
        flex-wrap: wrap;
        margin-top: 1.5rem;
        > div {
            outline: none;
            background-color: white;
            border: 2px solid #707070;
            border-radius: 3.125rem;
            margin: 0 1rem 0.75rem 0;
            padding: 0.375rem 2rem;
            color: var(--secondary-color);
            cursor: pointer;
            font: {
                size: 3.5em;
                weight: 200;
                style: italic
            }
            &:last-child {
                margin-right: 0;
            }
            &:hover {
                opacity: 0.85;
            }
        }
        > .skillSelected {
            border-color: white;
            background-color: var(--secondary-color);
            color: white;
        } 
    }

    #tagsGrid {
        position: relative;
        width: 100%;
        display: flex;
        flex-wrap: wrap;
        border: 2px solid white;
        border-radius: 0.625em;
        margin-top: 1rem;
        padding: 0.625rem;
        background-color: #358101;
        transition: height 0.25s;
        > .tag {
            font-size: 3em;
            margin: 0.5rem;
        }
    }

    @include for-desktop-large {
        #skills > h1 {
            font-size: 7vw;
        }

        .skillsBar > div {
            font-size: 4vw;
            border-radius: 5rem;
            border-width: 3px;
        }

        #tagsGrid > .tag {
            font-size: 4vw;
            margin: 1rem;
            border-radius: 0.625rem;
        }
    }

    @include for-desktop-xlarge {
        #skills > h1 {
            border-width: 5px;
        }

        .skillsBar > div {
            border-width: 5px;
        }
        #tagsGrid {
            border: {
                width: 5px;
                radius: 1.5rem;
            }
        }
    }

    @include for-desktop-only {
        .skillsBar {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 0.5rem;
            > div {
                width: 100%;
                text-align: center;
                padding: 0;
            }
        }
    }

    @include for-tablet-only {
        .skillsBar > div {
            font-size: 9vw;
        }
        #tagsGrid > .tag {
            font-size: 5vw;
            margin: 0.25rem;
            padding: 0.5rem;
        }
    }

    @include for-mobile-only {
        #skills { 
            width: 95%; 
            > h1 { font-size: 15vw; }
        }
    }
</style>