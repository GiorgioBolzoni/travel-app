<template>
    <section id="filter">
        <div class="d-flex justify-content-center">
            <select name="travel" id="travel"  class="form-select">
                <option 
                
                value="all">All</option>
            </select>
        </div>
    </section>

    <section id="travel-map" class="py-4">
        <div class="card p-4">
            travel
        </div>
    </section>
</template>


<script>
    export default {
        name: 'Main',
        data() {
            return{
                menu: [
                    {name: 'All', value:'All'},
                    {name: 'Italy', value:'Italy'},
                    {name: 'Scotland', value:'Scotland'},
                    {name: 'Thailand', value:'Thailand'},
                ],
            }
        }
    }
</script>


<style lang="scss" scoped>
    select{
        width: 100px;
        &:hover{
            cursor: pointer;
        };
        option{
            &:hover{
                cursor: pointer !important;
            }
        }
    }
</style>