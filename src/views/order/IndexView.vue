
<script setup>
	import useCategoryApi from "@/composables/categories/instance";
	import { onMounted, toRefs } from "vue";
	import MakeOrder from "@/components/adv/make-order.vue";
	const props = defineProps({
		categorySlug: {
			type: String,
			required: true,
		},
	});
	const { categorySlug } = toRefs(props);

	const { category, isLoading, isLoaded, promise, onGet, onGetWhenNotLoaded } =
		useCategoryApi();

	onMounted(async () => {
		await onGetWhenNotLoaded(categorySlug.value, {
			with: ["priority_services", "image"],
		});
	});
</script>
<template>
	<div
		class="
			container
			mx-auto
			gap-4
			py-4
			grid grid-cols-1
			md:grid-cols-7
			px-4
		"
	>
		<div class="md:col-span-7"></div>
		<div class="md:col-span-5 w-full">
			<router-view v-slot="{ Component }">
				<template v-if="Component">
					<KeepAlive>
						<Suspense>
							<Component :is="Component"></Component>
							<template #fallback>
								<div
									class="
										flex flex-col
										relative
										w-full
										min-h-full
										rounded-lg
										bg-white
										shadow shadow-primary/25
										overflow-clip
									"
								>
									<div class="p-4">
										<div class="pt-1.5 pb-0.5">
											<div
												class="
													h-7
													w-96
													inline-block
													bg-primary/10
													rounded-lg
													animate-pulse
												"
											></div>
										</div>
										<div class="pt-1.5 pb-0.5">
											<div
												class="
													h-4
													w-64
													bg-primary/10
													rounded-md
													animate-pulse
												"
											></div>
										</div>
									</div>

									<div
										class="px-4 flex-1 min-h-[18rem]"
									></div>

									<div
										class="
											p-4
											sticky
											bottom-0
											bg-white
											flex
											justify-between
											z-40
										"
									>
										<slot name="footer">
											<c-button
												scheme="secondary"
												type="button"
												disabled
												loading
											>
												????????????
											</c-button>
											<c-button disabled loading>
												????????????????????
											</c-button>
										</slot>
									</div>
								</div>
							</template>
						</Suspense>
					</KeepAlive>
				</template>
			</router-view>
		</div>
		<div class="md:col-span-2 w-full">
			<div class="rounded-lg bg-white shadow shadow-primary/25 p-4">
				<!-- <button
                    class="
                        block
                        px-4
                        p-2
                        bg-blue-900
                        w-full
                        rounded-lg
                        text-left
                        font-medium
                        shadow
                        transition
                        duration-200
                        text-white
                    "
                >
                    ??????????
                </button> -->
				<router-link
					to="/faq"
					class="hover:text-blue-600 text-lg mb-2 block"
				>
					???????????? ??????????????
				</router-link>
				<a
					href="javascript:jivo_api.open()"
					class="hover:text-blue-600 text-lg mb-2 block"
				>
					??????????????????
				</a>
			</div>

			<make-order
				class="hidden md:block"
				:category="categorySlug"
				:index="0"
			></make-order>
			<make-order
				class="hidden md:block"
				:category="categorySlug"
				:index="2"
			></make-order>
		</div>
		<div class="md:hidden grid grid-cols-2 gap-4">
			<make-order
				:category="categorySlug"
				:index="0"
				class="col-span-2 sm:col-span-1"
			></make-order>
			<make-order
				:category="categorySlug"
				:index="2"
				class="col-span-2 sm:col-span-1"
			></make-order>
		</div>
		<!-- <div class="basis-5/7 bg-white shadow-md rounded-lg p-4">
            <div></div>
            <div v-show="true">
                <div>
                    <div class="md:flex">
                        <div class="flex items-center mt-2 mr-8">
                            <span
                                class="
                                    bg-blue-100
                                    rounded-full
                                    h-7
                                    w-7
                                    flex
                                    items-center
                                    justify-center
                                    mr-4
                                "
                            >
                                <i
                                    class="
                                        icon-settings
                                        text-sm
                                        md:text-lg
                                        xl:text-xl
                                        2xl:text-2xl
                                    "
                                ></i>
                            </span>
                            <span
                                class="
                                    text-sm
                                    md:text-md
                                    xl:text-lg
                                    2xl:text-xl
                                    font-medium
                                    text-blue-950
                                "
                            >
                                ??????????????????
                            </span>
                        </div>
                        <div class="flex items-center mt-2">
                            <span
                                class="
                                    bg-blue-100
                                    rounded-full
                                    h-7
                                    w-7
                                    flex
                                    items-center
                                    justify-center
                                    mr-4
                                "
                            >
                                <i
                                    class="
                                        icon-location
                                        text-sm
                                        md:text-lg
                                        xl:text-xl
                                        2xl:text-2xl
                                    "
                                ></i>
                            </span>
                            <span
                                class="
                                    text-sm
                                    md:text-md
                                    xl:text-lg
                                    2xl:text-xl
                                    font-medium
                                    text-blue-950
                                "
                            >
                                ??. ??????????????, ?????????? ????????????????, 41
                            </span>
                        </div>
                    </div>
                    <div class="mt-4 flex items-center">
                        <span
                            class="
                                text-xs
                                sm:text-sm
                                md:text-lg
                                lg:text-base
                                2xl:text-lg
                                text-gray-400
                                mr-4
                            "
                        >
                            ????????????????????:
                        </span>
                        <div class="relative text-left">
                             v-for="(label, value) in "
                            <c-dropdown
                                v-model="sortBy"
                                :items="[
                                    { value: 'suitable', label: '????????????????????' },
                                    { value: 'onReviews', label: '???? ??????????????' },
                                ]"
                            >
                            </c-dropdown>
                        </div>
                    </div>
                    <hr class="border-gray-500 border-dashed my-4 md:my-8" />
                </div>

                <div class="md:grid md:grid-cols-3">
                    <div class="col-span-2">
                         ??????????????????????  
                        <div>
                            <div class="flex items-center">
                                <div class="mr-8">
                                    <img
                                        src="{{'assets/images/userlistchatbig.png' | theme}}"
                                        alt=""
                                    />
                                </div>

                                <div class="">
                                    <h5
                                        class="
                                            font-bold
                                            text-sm
                                            md:text-md
                                            lg:text-base
                                            2xl:text-lg
                                        "
                                    >
                                        ???????????? ??????????????
                                    </h5>
                                    <span
                                        class="
                                            text-xs
                                            md:text-sm
                                            lg:text-md
                                            2xl:text-base
                                            text-gray-400
                                        "
                                    >
                                        ?????? ?? ???????? ?????????????? ?? 12:20
                                    </span>
                                    <div
                                        class="
                                            flex
                                            justify-start
                                            text-xs
                                            md:text-sm
                                            lg:text-md
                                            2xl:text-base
                                            text-gray-400
                                            font-semibold
                                            my-1
                                        "
                                    >
                                        <div>
                                            <i class="icon-orangestar mr-2"></i>
                                            <span>10 ???? 10</span>
                                        </div>
                                        <div>
                                            <i class="icon-comments mx-2"></i>
                                            <span> 20 </span>
                                        </div>
                                        <div>
                                            <i class="icon-like mx-2"></i>
                                            <span> ?????????? ???????????? </span>
                                        </div>
                                    </div>
                                    <div
                                        class="
                                            my-1
                                            text-xs
                                            md:text-sm
                                            lg:text-md
                                            2xl:text-base
                                        "
                                    >
                                        <span
                                            class="
                                                font-medium
                                                text-sm
                                                md:text-md
                                            "
                                        >
                                            ?????????? ????
                                            <a
                                                class="
                                                    text-blue-950
                                                    transition-all
                                                    hover:underline
                                                    duration-500
                                                "
                                                href="#"
                                                >????????????????,</a
                                            >
                                            <a
                                                class="
                                                    text-blue-950
                                                    transition-all
                                                    hover:underline
                                                    duration-500
                                                "
                                                href="#"
                                            >
                                                ?????????????????????? ??????.</a
                                            >
                                        </span>
                                    </div>
                                </div>
                            </div>

                            <div class="mt-4">
                                <div class="">
                                    <h4
                                        class="
                                            text-sm
                                            md:text-md
                                            lg:text-base
                                            2xl:text-lg
                                            font-semibold
                                            text-orange-500
                                        "
                                    >
                                        ???????????????????????? ????????????????????????
                                    </h4>
                                    <p
                                        class="
                                            text-xs
                                            sm:text-sm
                                            lg:text-md
                                            2xl:text-base
                                        "
                                    >
                                        ???????????????????? ???????? ?????????????? ???? ??????????????:
                                        ??????????????????, ?????????????? ?????????????????????? ??????????.
                                    </p>
                                </div>
                                <div
                                    class="flex flex-wrap justify-between my-4"
                                >
                                    <img
                                        class="my-1"
                                        src="{{'assets/images/groupImg.png' | theme}}"
                                        alt=""
                                    />
                                    <img
                                        class="my-1"
                                        src="{{'assets/images/groupImg2.png' | theme}}"
                                        alt=""
                                    />
                                    <img
                                        class="my-1"
                                        src="{{'assets/images/groupImg3.png' | theme}}"
                                        alt=""
                                    />
                                    <img
                                        class="my-1"
                                        src="{{'assets/images/groupImg4.png' | theme}}"
                                        alt=""
                                    />
                                </div>
                                <div class="my-4">
                                    <h5
                                        class="
                                            text-sm
                                            md:text-md
                                            lg:text-base
                                            2xl:text-lg
                                            font-medium
                                        "
                                    >
                                        ?? ????????:
                                    </h5>
                                    <p
                                        class="
                                            text-xs
                                            sm:text-sm
                                            lg:text-md
                                            2xl:text-basel
                                        "
                                    >
                                        ????????????-????????????????,?????????????? ???????? ?? ??????????????
                                        ?????????? ???????????????????????????????? ??????????. ????????????
                                        ???????? ???? ?????????????????? ???????? ?????????????? ??????
                                        ?????????????????????? ???????????? ????????????????????????????.
                                        ????????????,?????????????????????? ???????????????????? ????????????????!
                                        ???????????? ???????????? ???????????? ?? ??????????????????????.
                                    </p>
                                    <div
                                        class="
                                            flex
                                            justify-between
                                            py-4
                                            border-b
                                            border-gray-500
                                            border-dotted
                                        "
                                    >
                                        <a
                                            class="
                                                text-xs
                                                sm:text-sm
                                                lg:text-md
                                                2xl:text-base
                                                text-blue-950
                                                transition
                                                duration-700
                                                ease-in-out
                                                hover:underline
                                            "
                                            href="#"
                                        >
                                            ??????????????????
                                        </a>
                                        <a href="#">
                                            <img
                                                class="w-5 h-5"
                                                src="{{'assets/images/googletranslate.png' | theme}}"
                                                alt=""
                                            />
                                        </a>
                                    </div>
                                </div>

                                <div class="my-4">
                                    <h5
                                        class="
                                            text-sm
                                            md:text-md
                                            lg:text-base
                                            2xl:text-lg
                                            font-medium
                                        "
                                    >
                                        ?????????????????????? ?? ????????:
                                    </h5>
                                    <p
                                        class="
                                            text-xs
                                            sm:text-sm
                                            lg:text-md
                                            2xl:text-base
                                        "
                                    >
                                        ?????????????????????? ?????????????????????????????? ??????????????????????
                                        ????????????????????, ?????????????? ?? ????????????????
                                        "?????????????????????????? ????????????" ?? 2010 ????
                                        2019??????, ???? ???????????? ???????????? ?????????????? ????
                                        ?????????? ???????????????? ????????????????.
                                    </p>
                                    <div
                                        class="
                                            flex
                                            justify-between
                                            py-4
                                            border-b
                                            border-gray-500
                                            border-dotted
                                        "
                                    >
                                        <a
                                            class="
                                                text-xs
                                                sm:text-sm
                                                lg:text-md
                                                2xl:text-base
                                                text-blue-950
                                                transition-all
                                                duration-500
                                                hover:underline
                                            "
                                            href="#"
                                        >
                                            ??????????????????
                                        </a>
                                        <a href="#">
                                            <img
                                                class="w-5 h-5"
                                                src="{{'assets/images/googletranslate.png' | theme}}"
                                                alt=""
                                            />
                                        </a>
                                    </div>
                                </div>

                                <div class="my-4">
                                    <h5
                                        class="
                                            text-sm
                                            md:text-md
                                            lg:text-base
                                            2xl:text-lg
                                            font-medium
                                        "
                                    >
                                        ????????????:
                                    </h5>
                                    <p
                                        class="
                                            text-xs
                                            sm:text-sm
                                            lg:text-md
                                            2xl:text-base
                                        "
                                    >
                                        ?????? ?????????? ?????? ????????????????. ???????????? ????????????????
                                        ?????????????????????????? ??????????????????????. ????????????
                                        ???????????????? ????????????, ?????????????????? ??????????????.
                                        ???????????? ????????????, ?????? ???????? ?????????????????????? ??????
                                        ???????????? ??????????????????, ???? ???????????? ?????????? ?? ????????
                                        ????????????????????.
                                    </p>
                                    <div
                                        class="
                                            flex
                                            justify-between
                                            py-4
                                            border-b
                                            border-gray-500
                                            border-dotted
                                        "
                                    >
                                        <a
                                            class="
                                                text-xs
                                                sm:text-sm
                                                lg:text-md
                                                2xl:text-base
                                                text-blue-950
                                                transition-all
                                                duration-500
                                                hover:underline
                                            "
                                            href="#"
                                        >
                                            ?????? ???????????? (20)
                                        </a>
                                        <a href="#">
                                            <img
                                                class="w-5 h-5"
                                                src="{{'assets/images/googletranslate.png' | theme}}"
                                                alt=""
                                            />
                                        </a>
                                    </div>
                                </div>

                                <div class="mt-4">
                                    <h5
                                        class="
                                            text-sm
                                            md:text-md
                                            lg:text-base
                                            2xl:text-lg
                                            font-medium
                                        "
                                    >
                                        ???????????? ?? ????????
                                    </h5>
                                    <div
                                        class="
                                            text-xs
                                            sm:text-sm
                                            lg:text-md
                                            2xl:text-base
                                        "
                                    >
                                        <ul>
                                            <li
                                                class="
                                                    flex
                                                    justify-between
                                                    my-1
                                                    border-b
                                                    border-gray-500
                                                    border-dotted
                                                "
                                            >
                                                <span class="-mb-1 bg-white"
                                                    >???????????? ????????????????</span
                                                >
                                                <span
                                                    class="
                                                        text-gray-400
                                                        font-medium
                                                        -mb-1
                                                        bg-white
                                                    "
                                                    >???? 25 000 ??????</span
                                                >
                                            </li>
                                            <li
                                                class="
                                                    flex
                                                    justify-between
                                                    my-1
                                                    border-b
                                                    border-gray-500
                                                    border-dotted
                                                "
                                            >
                                                <span class="-mb-1 bg-white"
                                                    >?????????????????? ??????????????, 1
                                                    ??????????</span
                                                >
                                                <span
                                                    class="
                                                        text-gray-400
                                                        font-medium
                                                        -mb-1
                                                        bg-white
                                                    "
                                                    >???? 15 000 ??????</span
                                                >
                                            </li>
                                            <li
                                                class="
                                                    flex
                                                    justify-between
                                                    my-1
                                                    border-b
                                                    border-gray-500
                                                    border-dotted
                                                "
                                            >
                                                <span class="-mb-1 bg-white"
                                                    >?????????????????? ??????????????????????</span
                                                >
                                                <span
                                                    class="
                                                        text-gray-400
                                                        font-medium
                                                        -mb-1
                                                        bg-white
                                                    "
                                                    >???? 10 000 ??????</span
                                                >
                                            </li>
                                            <li
                                                class="
                                                    flex
                                                    justify-between
                                                    my-1
                                                    border-b
                                                    border-gray-500
                                                    border-dotted
                                                "
                                            >
                                                <span class="-mb-1 bg-white"
                                                    >?????????????????? ??????????????????????</span
                                                >
                                                <span
                                                    class="
                                                        text-gray-400
                                                        font-medium
                                                        -mb-1
                                                        bg-white
                                                    "
                                                    >???? 25 000 ??????</span
                                                >
                                            </li>
                                        </ul>
                                    </div>
                                    <div class="flex justify-between pt-4">
                                        <a
                                            class="
                                                text-xs
                                                sm:text-sm
                                                lg:text-md
                                                2xl:text-base
                                                text-blue-950
                                                transition-all
                                                duration-500
                                                hover:underline
                                            "
                                            href="#"
                                        >
                                            ?????? ???????????? ?? ???????? (18)
                                        </a>
                                        <a href="#">
                                            <img
                                                class="w-5 h-5"
                                                src="{{'assets/images/googletranslate.png' | theme}}"
                                                alt=""
                                            />
                                        </a>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>

                    <div class="col-span-1">
                        <div class="w-full px-3">
                            <a
                                href="#"
                                class="
                                    bg-blue-975
                                    border
                                    transition
                                    duration-500
                                    border-blue-975
                                    rounded-md
                                    block
                                    w-full
                                    py-3
                                    text-center text-white text-sm
                                    sm:text-sm
                                    md:text-md
                                    lg:text-base
                                    2xl:text-lg
                                    font-medium
                                    hover:text-blue-950 hover:bg-white
                                "
                            >
                                ???????????????? ??????????????????
                            </a>
                            <p
                                class="
                                    my-2
                                    text-xs
                                    sm:text-sm
                                    lg:text-md
                                    2xl:text-base
                                    text-center
                                "
                            >
                                ???? ?????????????? ???????????????? ?????????? ???????????????????? ??????????????
                                ????????????
                            </p>
                        </div>
                    </div>
                </div>
            </div> 
        </div> -->
		<!-- <div class="basis-2/7 flex flex-col">
            <div class="bg-white shadow-md rounded-lg p-4 flex flex-col gap-4">
                <button
                    class="
                        block
                        px-4
                        p-2
                        bg-blue-900
                        w-full
                        rounded-lg
                        text-left
                        font-medium
                        shadow
                        transition
                        duration-200
                    "
                >
                    ??????????
                </button>
                <button
                    class="
                        block
                        px-4
                        p-2
                        bg-blue-900
                        w-full
                        rounded-lg
                        text-left
                        font-medium
                        shadow
                        transition
                        duration-200
                    "
                >
                    ??????????
                </button>
            </div>
        </div> -->
	</div>
</template>