# vue-slide-menu

## Install
```
npm install vue-slide-menu --save
```

## Importing Component
```
<script>
import VueSlideMenu from 'vue-slide-menu';

export default {
    components: {
        'vue-slide-menu': VueSlideMenu,
    },
}
</script>
```

## Importing Styles
```
<style>
@import '~vue-slide-menu/dist/vue-slide-menu.css';
</style>
```

## Usage
```
<vue-slide-menu title="MenuTitle" :open.sync="myMenuOpenVar" :menu="myMenuVar" />

// ...

data() {
    return {
        myMenuOpenVar: false,
        myMenuVar: [
            {
                name: 'Main Menu Entry',
                value: [
                    {
                        name: 'SubHeading',
                        value: [
                            { name: 'My Link', value: '/index.html' },
                        ],
                    },
                ],
            }
        ]
    };
}
```