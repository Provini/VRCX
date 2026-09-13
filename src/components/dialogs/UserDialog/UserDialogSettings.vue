<template>
    <Popover>
        <PopoverTrigger asChild>
            <Button variant="ghost" size="icon" class="size-7" :ariaLabel="t('dialog.user.settings.header')">
                <Settings :color="userDialog.theme.iconColor" class="size-4" />
            </Button>
        </PopoverTrigger>
        <PopoverContent side="bottom" align="end" class="w-82 flex flex-col gap-3">
            <div class="flex flex-col gap-2.5">
                <span class="text-xs font-semibold text-muted-foreground uppercase tracking-wide">{{
                    t('dialog.user.settings.profile_display')
                }}</span>

                <Field orientation="horizontal">
                    <FieldLabel>{{ t('view.settings.appearance.appearance.vrc_profile_themes') }}</FieldLabel>
                    <Switch
                        :model-value="displayVRCProfileThemes"
                        :ariaLabel="t('view.settings.appearance.appearance.vrc_profile_themes')"
                        @update:modelValue="setDisplayVRCProfileThemes" />
                </Field>

                <Field orientation="horizontal">
                    <FieldLabel>
                        <span class="flex items-center gap-1">
                            <span>{{ t('view.settings.appearance.appearance.vrc_profile_backgrounds') }}</span>
                            <TooltipWrapper
                                side="top"
                                :content="t('view.settings.appearance.appearance.vrc_profile_backgrounds_description')">
                                <Info
                                    class="text-muted-foreground"
                                    ariaLabel="t('view.settings.appearance.appearance.vrc_profile_backgrounds_description')" />
                            </TooltipWrapper>
                        </span>
                    </FieldLabel>
                    <Switch
                        :model-value="displayVRCProfileBackgrounds"
                        :ariaLabel="t('view.settings.appearance.appearance.vrc_profile_backgrounds')"
                        @update:modelValue="setDisplayVRCProfileBackgrounds" />
                </Field>

                <Field v-if="displayVRCProfileBackgrounds" orientation="horizontal">
                    <FieldLabel>
                        <span class="flex items-center gap-1">
                            <span>{{ t('view.settings.appearance.appearance.vrc_profile_backgrounds_opacity') }}</span>
                            <TooltipWrapper
                                side="top"
                                :content="
                                    t('view.settings.appearance.appearance.vrc_profile_backgrounds_opacity_description')
                                ">
                                <Info
                                    class="text-muted-foreground"
                                    ariaLabel="t('view.settings.appearance.appearance.vrc_profile_backgrounds_opacity_description')" />
                            </TooltipWrapper>
                        </span>
                    </FieldLabel>
                    <NumberField
                        :model-value="profileBackgroundOpacity"
                        :step="0.1"
                        :min="0"
                        :max="1"
                        :format-options="{ maximumFractionDigits: 2 }"
                        class="w-20"
                        @update:modelValue="setProfileBackgroundOpacity">
                        <NumberFieldContent>
                            <NumberFieldDecrement class="p-1.5" />
                            <NumberFieldInput class="h-7" />
                            <NumberFieldIncrement class="p-1.5" />
                        </NumberFieldContent>
                    </NumberField>
                </Field>

                <Field orientation="horizontal">
                    <FieldLabel>
                        <span class="flex items-center gap-1">
                            <span>{{ t('view.settings.appearance.appearance.vrc_profile_card_opacity') }}</span>
                            <TooltipWrapper
                                side="top"
                                :content="
                                    t('view.settings.appearance.appearance.vrc_profile_card_opacity_description')
                                ">
                                <Info
                                    class="text-muted-foreground"
                                    ariaLabel="t('view.settings.appearance.appearance.vrc_profile_card_opacity_description')" />
                            </TooltipWrapper>
                        </span>
                    </FieldLabel>
                    <NumberField
                        :model-value="profileCardOpacity"
                        :step="0.1"
                        :min="0"
                        :max="1"
                        :format-options="{ maximumFractionDigits: 2 }"
                        class="w-20"
                        @update:modelValue="setProfileCardOpacity">
                        <NumberFieldContent>
                            <NumberFieldDecrement class="p-1.5" />
                            <NumberFieldInput class="h-7" />
                            <NumberFieldIncrement class="p-1.5" />
                        </NumberFieldContent>
                    </NumberField>
                </Field>

                <Field orientation="horizontal">
                    <FieldLabel>
                        <span class="flex items-center gap-1">
                            <span>{{ t('view.settings.appearance.appearance.vrc_profile_cosmetics') }}</span>
                            <TooltipWrapper
                                side="top"
                                :content="t('view.settings.appearance.appearance.cosmetics_description')">
                                <Info
                                    class="text-muted-foreground"
                                    :ariaLabel="t('view.settings.appearance.appearance.cosmetics_description')" />
                            </TooltipWrapper>
                        </span>
                    </FieldLabel>
                    <Switch
                        :model-value="displayVRCProfileCosmetics"
                        :ariaLabel="t('view.settings.appearance.appearance.vrc_profile_cosmetics')"
                        @update:modelValue="setDisplayVRCProfileCosmetics" />
                </Field>
            </div>

            <Separator />

            <Collapsible class="flex flex-col gap-2.5" v-model:open="isCollapsableOpen">
                <CollapsibleTrigger as-child>
                    <button
                        type="button"
                        class="flex w-full items-center justify-between py-0.5 text-[11px] font-medium text-muted-foreground uppercase tracking-wide cursor-pointer hover:text-foreground transition-colors cursor-pointer">
                        {{ t('dialog.user.settings.notes_and_actions') }}
                        <ChevronDown
                            class="size-3.5 transition-transform duration-200"
                            :class="{ 'rotate-180': isCollapsableOpen }" />
                    </button>
                </CollapsibleTrigger>
                <CollapsibleContent class="flex flex-col gap-2.5 overflow-hidden">
                    <Field orientation="horizontal">
                        <FieldLabel>
                            <span class="flex items-center gap-1">
                                <span>{{ t('view.settings.appearance.user_dialog.vrchat_notes') }}</span>
                                <TooltipWrapper
                                    side="top"
                                    :content="t('view.settings.appearance.user_dialog.vrchat_notes_description')">
                                    <Info
                                        class="text-muted-foreground"
                                        :ariaLabel="
                                            t('view.settings.appearance.user_dialog.vrchat_notes_description')
                                        " />
                                </TooltipWrapper>
                            </span>
                        </FieldLabel>
                        <Switch
                            :model-value="!hideUserNotes"
                            :ariaLabel="t('view.settings.appearance.user_dialog.vrchat_notes')"
                            @update:modelValue="setHideUserNotes" />
                    </Field>

                    <Field orientation="horizontal">
                        <FieldLabel>
                            <span class="flex items-center gap-1">
                                <span>{{ t('view.settings.appearance.user_dialog.vrcx_memos') }}</span>
                                <TooltipWrapper
                                    side="top"
                                    :content="t('view.settings.appearance.user_dialog.vrcx_memos_description')">
                                    <Info
                                        class="text-muted-foreground"
                                        :ariaLabel="t('view.settings.appearance.user_dialog.vrcx_memos_description')" />
                                </TooltipWrapper>
                            </span>
                        </FieldLabel>
                        <Switch
                            :model-value="!hideUserMemos"
                            :ariaLabel="t('view.settings.appearance.user_dialog.vrcx_memos')"
                            @update:modelValue="setHideUserMemos" />
                    </Field>

                    <Field orientation="horizontal">
                        <FieldLabel>
                            <span class="flex items-center gap-1">
                                <span>{{ t('view.settings.appearance.user_dialog.recent_action_cooldown') }}</span>
                                <TooltipWrapper
                                    side="top"
                                    :content="
                                        t('view.settings.appearance.user_dialog.recent_action_cooldown_description')
                                    ">
                                    <Info
                                        class="text-muted-foreground"
                                        :ariaLabel="
                                            t('view.settings.appearance.user_dialog.recent_action_cooldown_description')
                                        " />
                                </TooltipWrapper>
                            </span>
                        </FieldLabel>
                        <Switch
                            :model-value="recentActionCooldownEnabled"
                            :ariaLabel="t('view.settings.appearance.user_dialog.recent_action_cooldown')"
                            @update:modelValue="setRecentActionCooldownEnabled" />
                    </Field>

                    <Field v-if="recentActionCooldownEnabled" orientation="horizontal">
                        <FieldLabel>{{
                            t('view.settings.appearance.user_dialog.recent_action_cooldown_minutes')
                        }}</FieldLabel>
                        <NumberField
                            :model-value="recentActionCooldownMinutes"
                            :min="1"
                            :max="1440"
                            :step="1"
                            :format-options="{ maximumFractionDigits: 0 }"
                            class="w-20"
                            @update:modelValue="setRecentActionCooldownMinutes">
                            <NumberFieldContent>
                                <NumberFieldDecrement class="p-1.5" />
                                <NumberFieldInput class="h-7" />
                                <NumberFieldIncrement class="p-1.5" />
                            </NumberFieldContent>
                        </NumberField>
                    </Field>
                </CollapsibleContent>
            </Collapsible>
        </PopoverContent>
    </Popover>
</template>

<script setup>
    import { watch, ref } from 'vue';
    import { Popover, PopoverContent, PopoverTrigger } from '@/components/ui/popover';
    import { Button } from '@/components/ui/button';
    import { Switch } from '@/components/ui/switch';
    import {
        NumberField,
        NumberFieldContent,
        NumberFieldDecrement,
        NumberFieldIncrement,
        NumberFieldInput
    } from '@/components/ui/number-field';
    import { ChevronDown, Info, Settings } from 'lucide-vue-next';
    import { Collapsible, CollapsibleContent, CollapsibleTrigger } from '@/components/ui/collapsible';
    import { Separator } from '@/components/ui/separator';
    import { Field, FieldLabel } from '@/components/ui/field';
    import { TooltipWrapper } from '@/components/ui/tooltip';
    import { storeToRefs } from 'pinia';
    import { useI18n } from 'vue-i18n';

    import { useAppearanceSettingsStore, useGeneralSettingsStore, useUserStore } from '../../../stores';
    import { getReadableProfileThemeColor } from '../../../shared/utils/user';

    const { t } = useI18n();
    const { userDialog } = storeToRefs(useUserStore());

    const appearanceSettingsStore = useAppearanceSettingsStore();
    const {
        displayVRCProfileThemes,
        displayVRCProfileBackgrounds,
        profileBackgroundOpacity,
        displayVRCProfileCosmetics,
        profileCardOpacity,
        hideUserNotes,
        hideUserMemos,
        isDarkMode
    } = storeToRefs(appearanceSettingsStore);
    const {
        setDisplayVRCProfileThemes,
        setDisplayVRCProfileBackgrounds,
        setProfileBackgroundOpacity,
        setDisplayVRCProfileCosmetics,
        setProfileCardOpacity,
        setHideUserNotes,
        setHideUserMemos
    } = appearanceSettingsStore;

    const generalSettingsStore = useGeneralSettingsStore();
    const { recentActionCooldownEnabled, recentActionCooldownMinutes } = storeToRefs(generalSettingsStore);
    const { setRecentActionCooldownEnabled, setRecentActionCooldownMinutes } = generalSettingsStore;
    const isCollapsableOpen = ref(false);

    watch([displayVRCProfileThemes, isDarkMode], () => {
        const profile = userDialog.value.publicProfileRef;
        if (!displayVRCProfileThemes.value || !profile) {
            userDialog.value.theme = {
                iconColor: 'var(--muted-foreground)',
                buttonColor: 'var(--primary)',
                subtextColor: 'var(--muted-foreground)'
            };
            return;
        }
        userDialog.value.theme = {
            iconColor: getReadableProfileThemeColor(
                profile.themeIconColor,
                'var(--muted-foreground)',
                isDarkMode.value
            ),
            buttonColor: getReadableProfileThemeColor(profile.themeButtonColor, 'var(--primary)', isDarkMode.value),
            subtextColor: getReadableProfileThemeColor(
                profile.themeSubtextColor,
                'var(--muted-foreground)',
                isDarkMode.value
            )
        };
    });
</script>
