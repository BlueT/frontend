<style scoped>
    pre {
        font-size: .7em;
    }
</style>
<template>
    <div id="entries-page">
        <div class="row">
            <div class="col-md-6">
                <div id="searchEntries">
                    <div class="input-group">
                        <span class="input-group-addon" id="search-addon">
                            <i class="fa fa-search" aria-hidden="true"></i>
                        </span>
                        <input type="text" class="form-control" placeholder="{{{ $t('entries.search') }}}"
                               v-model="search" aria-describedby="search-addon"
                               @keyup="filterEntry(search) | debounce 500">
                    </div>
                </div>
            </div>
            <div class="col-md-6 text-xs-right">
                <new-entry-button></new-entry-button>
            </div>
        </div>

        <div class="row m-y-2" v-show="!entries.length">
            <help-component></help-component>
            <div class="col-lg-4 col-md-6">
                <div class="card">
                    <div class="card-header">
                        <i class="fa fa-key" aria-hidden="true"></i> {{{ $t('help.letsGo') }}}
                    </div>
                    <div class="card-block">
                        <p class="card-text">
                            {{{ $t('help.letsGo1') }}}
                            <new-entry-button></new-entry-button>
                        </p>
                    </div>
                </div>
            </div>
        </div>
        <div class="row m-y-2">
            <div class="col-lg-12">
                <div class="card-columns">
                    <div v-for="entry in entries">
                        <lesspass-entry :entry="entry"></lesspass-entry>
                    </div>
                </div>
            </div>
        </div>
        <div class="row m-t-1">
            <div class="paginate">
                <div class="col-xs-4 text-xs-left">
                    <button class="btn btn-primary btn-sm" v-if="count > limit"
                            :disabled="(currentPage===1)"
                            @click="getNextEntries()">
                        {{{ $t('entries.previous') }}}
                    </button>
                </div>
                <div class="col-xs-4 text-xs-center" v-if="numberPages > 1">
                    {{ currentPage }} / {{ numberPages }}
                </div>
                <div class="col-xs-4 text-xs-right">
                    <button class="btn btn-primary btn-sm" v-if="count > limit"
                            :disabled="(currentPage*limit >= count)"
                            @click="getPreviousEntries()">
                        {{{ $t('entries.next') }}}
                    </button>
                </div>
            </div>
        </div>
        <copy-password-modal :entry="selectedEntry"></copy-password-modal>
    </div>
</template>
<script type="text/ecmascript-6">
    import 'bootstrap/dist/js/umd/modal';
    import Entries from '../services/entries';
    import LesspassEntry from '../components/entry.vue';
    import NewEntryButton from '../components/new-entry-button';
    import HelpComponent from '../components/help';
    import CopyPasswordModal from '../components/copy-password-modal';
    Entries.localStorage = localStorage;
    export default {
        data() {
            return {
                limit: 12,
                offset: 0,
                currentPage: 1,
                entries: [],
                selectedEntry: null,
                numberPages: 1,
                count: 0,
                clicks: 0
            };
        },
        components: {
            LesspassEntry,
            HelpComponent,
            NewEntryButton,
            CopyPasswordModal
        },
        ready(){
            this.getEntries(this.limit, this.offset);
        },
        methods: {
            getEntries(limit, offset, search = ''){
                Entries.all(limit, offset, search).then(response => {
                    this.entries = response.data.results;
                    this.count = response.data.count;
                    this.numberPages = Math.ceil(this.count / this.limit);
                })
            },
            getPreviousEntries() {
                this.currentPage += 1;
                this.offset = (this.currentPage - 1) * this.limit;
                this.getEntries(this.limit, this.offset);
            },
            getNextEntries() {
                this.currentPage -= 1;
                this.offset = (this.currentPage - 1) * this.limit;
                this.getEntries(this.limit, this.offset);
            },
            filterEntry(query){
                this.getEntries(this.limit, this.offset, query);
            }
        },
        events: {
            'edit-entry': function (entry) {
                this.$router.go(`/entries/${entry.id}/`);
            },
            'generate-password': function (entry) {
                this.selectedEntry = entry;
                $('#copyPasswordModal').modal('show');
            }
        }
    };
</script>
