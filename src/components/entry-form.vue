<template>
    <form id="password-generator-form">
        <div class="form-group row">
            <div class="col-lg-6 m-t-1">
                <label for="pg-login" class="sr-only">
                    {{ $t('entries.login') }}
                </label>
                <input id="pg-login"
                       class="form-control"
                       type="text"
                       placeholder="{{ $t('entries.login') }}"
                       value="{{entry.login}}"
                       v-model="entry.login"
                       autofocus
                       autocomplete="off"
                       autocorrect="off"
                       autocapitalize="none">
            </div>
        </div>
        <div class="form-group row">
            <div class="col-lg-12">
                <label for="pg-site" class="sr-only">
                    {{ $t('entries.site') }}
                </label>
                <input id="pg-site"
                       class="form-control"
                       list="domains"
                       type="text"
                       placeholder="{{ $t('entries.site') }}"
                       value="{{entry.site}}"
                       v-model="entry.site"
                       autocorrect="off"
                       autocapitalize="none">
                <datalist id="domains">
                    <option v-for="domain in domains" v-bind:value="domain">
                </datalist>
            </div>
        </div>
        <div class="form-group row m-b-0">
            <div class="col-lg-12">
                <div class="row">
                    <div class="col-lg-5">
                        <label class="c-input c-checkbox">
                            <input type="checkbox" id="lowercase" value="lowercase"
                                   v-model="entry.password.settings">
                            <span class="c-indicator"></span>
                            {{ $t('entries.lowercaseOptions') }}
                        </label>
                    </div>
                    <div class="col-lg-7">
                        <label class="c-input c-checkbox">
                            <input type="checkbox" id="uppercase" value="uppercase"
                                   v-model="entry.password.settings">
                            <span class="c-indicator"></span>
                            {{ $t('entries.uppercaseOptions') }}
                        </label>
                    </div>
                </div>
                <div class="row">
                    <div class="col-lg-5">
                        <label class="c-input c-checkbox">
                            <input type="checkbox" id="numbers" value="numbers"
                                   v-model="entry.password.settings">
                            <span class="c-indicator"></span>
                            {{ $t('entries.numbersOptions') }}
                        </label>
                    </div>
                    <div class="col-lg-7">
                        <label class="c-input c-checkbox">
                            <input type="checkbox" id="symbols" value="symbols"
                                   v-model="entry.password.settings">
                            <span class="c-indicator"></span>
                            {{ $t('entries.symbolsOptions') }}
                        </label>
                    </div>
                </div>
                <div class="row m-t-1">
                    <div class="col-lg-5 m-b-1">
                        <label for="passwordLength" class="sr-only">
                            {{ $t('entries.length') }}
                        </label>
                        <div class="input-group">
                            <span class="input-group-addon" id="passwordLengthAddon">
                                {{ $t('entries.length') }}
                            </span>
                            <input type="number" class="form-control" id="passwordLength"
                                   aria-describedby="passwordLengthAddon"
                                   v-model="entry.password.length"
                                   value="{{entry.password.length}}" min="6" max="64">
                        </div>
                    </div>
                    <div class="col-lg-4 m-b-1">
                        <label for="passwordCounter" class="sr-only">
                            {{ $t('entries.counter') }}
                        </label>
                        <div class="input-group">
                            <span class="input-group-addon" id="passwordCounterAddon">
                                {{ $t('entries.counter') }}
                            </span>
                            <input type="number" class="form-control" id="passwordCounter"
                                   aria-describedby="passwordCounterAddon"
                                   v-model="entry.password.counter"
                                   value="{{entry.password.counter}}" min="1" max="100">
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </form>
</template>
<script type="text/ecmascript-6">
    export default {
        data() {
            return {
                domains: []
            };
        },
        props: ['entry']
    };
</script>
