<template>
    <div class="col" :class="colClass"
        :style="colStyle">
        <div style="border: 1px solid green; height: 100px;">
          <slot></slot>
        </div>
    </div>
</template>
<script>
let validator = (value) => {
   let keys = Object.keys(value)
   let valid = true
   keys.forEach( key => {
       if (!['span', 'offset'].includes(key)){
           valid = false
       }
   })
   return valid
}
export default {
    name: "GuluCol",
    props: {
        span: {
            type: [Number, String]
        },
        offset: {
            type: [Number, String]
        },
        phone: {
            type: Object,
            validator,
        },
        ipad: {
            type: Object,
            validator,
        },
        narrowPc: {
            type: Object,
            validator,
        },
        pc: {
            type: Object,
            validator,
        },
        widePc: {
            type: Object,
            validator,
        },
    },
    data () {
        return {
            gutter: 0
        }
    },
    computed: {
        colClass () {
          let {span, offset,phone,ipad,narrowPc,pc,widePc} = this
          let phoneClass = []
          return [span && `col-${span}`, 
          offset && `offset-${offset}`,
          ... (phone ? [`col-phone-${phone.span}`] : []),
          ... (ipad ? [`col-ipad-${ipad.span}`] : []),
          ... (narrowPc ? [`col-narrow-pc-${narrowPc.span}`] : []),
          ... (pc ? [`col-pc-${pc.span}`] : []),
          ... (widePc ? [`col-wide-pc-${widePc.span}`] : []),
          ]
        },
        colStyle () {
           let {gutter} = this
          return {paddingLeft: gutter/2+'px', paddingRight: gutter/2+'px'}
        }
    }
}
</script>

<style scoped lang="scss">
    .col {
        width: 50%;
        $class: col-;
        @for $n from 1 through 24 {
            &.#{$class}#{$n} {
                width: ($n / 24) * 100%;
            }
        }

        $class: offset-;
        @for $n from 1 through 24 {
            &.#{$class}#{$n} {
                margin-left: ($n / 24) * 100%;
            }
        }

        @media (max-width: 576px) {
            $class: col-phone-;
            @for $n from 1 through 24 {
                &.#{$class}#{$n} {
                    width: ($n / 24) * 100%;
                }
            }
            $class: offset-phone-;
            @for $n from 1 through 24 {
                &.#{$class}#{$n} {
                    width: ($n / 24) * 100%;
                }
            }
        }
    }
</style>
