# Hyperway
 
## Install

`npm i -S hyperway`

## Usage

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <script type="module">
        import { h, text, app } from "https://unpkg.com/hyperapp"
        import { Hyperway, routeTo } from 'https://unpkg.com/hyperway'

        let Link = ({ to }, t) => h('div', { onclick: routeTo(to) }, text(t))

        app({
            init: {
                url: '',
                params: {}
            },
            view: state => h('div', {}, [
                h('div', {}, [text(`You are on "${state.url}" and params are ${JSON.stringify(state.params)}`)]),
                Link({ to: '/' }, 'Go to /'),
                Link({ to: '/users/123/delete' }, 'Go to /users/123/delete'),
                Link({ to: '/users/123' }, 'Go to /users/123'),
                Link({ to: '/public/img/logo.png' }, 'Go to /public/img/logo.png')
            ]),
            subscriptions: state => [
                Hyperway({
                    onNotFound: (state, props) => {
                        console.log('NOT FOUND', props)
                        return state
                    },
                    onRoute: (state, props) => {
                        console.log('ROUTE', props)
                        return { ...state, url: props.path, params: props.params }
                    },
                    routes: [
                        {
                            // page path (required)
                            path: '/',
                            // optional
                            onEnter: (state, props) => {
                                console.log('enter /', props)
                                return state
                            },
                            // optional
                            onLeave: (state, props) => {
                                console.log('leave /', props)
                                return state
                            }
                        },
                        {
                            // path with params
                            path: '/users/:id/:action',
                            onEnter: (state, props) => {
                                console.log('"id" and "action" are required', props.params)
                                return state
                            }
                        },
                        {
                            // optional param
                            path: '/users/:id/:action?',
                            onEnter: (state, props) => {
                                console.log('required "id" and optional "action"', props.params)
                                return state
                            }
                        },
                        {
                            // wildcard
                            path: '/public/*',
                            onEnter: (state, props) => {
                                console.log('wildcard', props.params)
                                return state
                            }
                        }
                    ]
                })
            ],
            node: document.getElementById('app')
        })
    </script>
</head>

<body>
    <main id="app"></main>
</body>

</html>
```