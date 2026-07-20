<script>
	import tailwindConfig from '../../../tailwind.config';
	import resolveConfig from 'tailwindcss/resolveConfig';
	import Katex from '~/utils/Katex.svelte';
</script>

<div id="description" dir="rtl">
	<div class="article-section" data-click="article-intro">
		<h1>ما هو المحوِّل (Transformer)؟</h1>

		<p>
			المحوِّل (Transformer) هو بنية شبكة عصبية غيّرت جذريًا نهج الذكاء الاصطناعي. قُدِّم المحوِّل
			لأول مرة في الورقة البحثية الرائدة
			<a
				href="https://dl.acm.org/doi/10.5555/3295222.3295349"
				title="ACM Digital Library"
				target="_blank">"Attention is All You Need"</a
			>
			عام 2017، وأصبح منذ ذلك الحين البنية المفضّلة لنماذج التعلم العميق، حيث يشغّل نماذج توليد
			النصوص مثل <strong>GPT</strong> من OpenAI و<strong>Llama</strong> من Meta و<strong
				>Gemini</strong
			> من Google. وإلى جانب النصوص، يُستخدم المحوِّل أيضًا في
			<a
				href="https://huggingface.co/learn/audio-course/en/chapter3/introduction"
				title="Hugging Face"
				target="_blank">توليد الصوت</a
			>
			و<a
				href="https://huggingface.co/learn/computer-vision-course/unit3/vision-transformers/vision-transformers-for-image-classification"
				title="Hugging Face"
				target="_blank">التعرف على الصور</a
			>
			و<a href="https://elifesciences.org/articles/82819" title="eLife"
				>التنبؤ ببنية البروتينات</a
			>
			وحتى في
			<a
				href="https://www.deeplearning.ai/the-batch/reinforcement-learning-plus-transformers-equals-efficiency/"
				title="Deep Learning AI"
				target="_blank">ممارسة الألعاب</a
			>، مما يدل على تعدد استخداماته في مجالات عديدة.
		</p>
		<p>
			في جوهرها، تعمل نماذج المحوِّلات المولِّدة للنص على مبدأ <strong
				>التنبؤ بالكلمة التالية (next-word prediction)</strong
			>: بالنظر إلى النص الذي يُدخله المستخدم، ما <em>الكلمة التالية الأكثر احتمالًا</em> بعد هذا
			الإدخال؟ يكمن الابتكار الجوهري وقوة المحوِّلات في استخدامها لآلية الانتباه الذاتي
			(self-attention)، التي تتيح لها معالجة التسلسلات كاملةً والتقاط العلاقات بعيدة المدى بفعالية
			أكبر من البنى السابقة.
		</p>
		<p>
			تُعد عائلة نماذج GPT-2 من الأمثلة البارزة على المحوِّلات المولِّدة للنص. يعمل «شارح
			المحوِّلات» بواسطة نموذج
			<a href="https://huggingface.co/openai-community/gpt2" title="Hugging Face" target="_blank"
				>GPT-2</a
			>
			(الصغير) الذي يضم 124 مليون مُعامِل. ورغم أنه ليس أحدث نماذج المحوِّلات أو أقواها، فإنه يشترك
			في كثير من المكونات والمبادئ المعمارية الموجودة في أحدث النماذج الحالية، مما يجعله نقطة
			انطلاق مثالية لفهم الأساسيات.
		</p>
	</div>

	<div class="article-section" data-click="article-overview">
		<h1>بنية المحوِّل (Transformer Architecture)</h1>

		<p>
			يتكوّن كل محوِّل مولِّد للنص من هذه <strong>المكونات الرئيسية الثلاثة</strong>:
		</p>
		<ol>
			<li>
				<strong class="bold-purple">التضمين (Embedding)</strong>: يُقسَّم النص المُدخل إلى وحدات
				أصغر تسمى الرموز (Tokens)، وقد تكون كلمات أو أجزاء كلمات. تُحوَّل هذه الرموز إلى متجهات
				عددية تسمى التضمينات (Embeddings)، تلتقط المعنى الدلالي للكلمات.
			</li>
			<li>
				<strong class="bold-purple">كتلة المحوِّل (Transformer Block)</strong> هي وحدة البناء
				الأساسية في النموذج التي تعالج البيانات المُدخلة وتحوِّلها. تتضمن كل كتلة:
				<ul class="">
					<li>
						<strong>آلية الانتباه (Attention Mechanism)</strong>، المكوّن الجوهري في كتلة
						المحوِّل. تتيح للرموز التواصل مع الرموز الأخرى، فتلتقط المعلومات السياقية والعلاقات
						بين الكلمات.
					</li>
					<li>
						<strong>طبقة الشبكة العصبية متعددة الطبقات (MLP)</strong>، وهي شبكة تغذية أمامية تعمل
						على كل رمز على حدة. بينما تهدف طبقة الانتباه إلى توجيه المعلومات بين الرموز، فإن هدف
						MLP هو صقل تمثيل كل رمز.
					</li>
				</ul>
			</li>
			<li>
				<strong class="bold-purple">احتمالات المخرجات (Output Probabilities)</strong>: تحوِّل
				الطبقة الخطية النهائية وطبقة softmax التضمينات المُعالجة إلى احتمالات، مما يمكّن النموذج
				من التنبؤ بالرمز التالي في التسلسل.
			</li>
		</ol>
	</div>

	<div class="article-section" id="embedding" data-click="article-embedding">
		<h2>التضمين (Embedding)</h2>
		<p>
			لنفترض أنك تريد توليد نص باستخدام نموذج محوِّل. تضيف نصًا مثل هذا:
			<code>“Data visualization empowers users to”</code>. يجب تحويل هذا الإدخال إلى صيغة يستطيع
			النموذج فهمها ومعالجتها. وهنا يأتي دور التضمين: فهو يحوِّل النص إلى تمثيل عددي يمكن للنموذج
			العمل به. لتحويل النص إلى تضمين، نحتاج إلى: 1) تقسيم الإدخال إلى رموز، 2) الحصول على تضمينات
			الرموز، 3) إضافة معلومات الموضع، وأخيرًا 4) جمع ترميزات الرموز والمواضع للحصول على التضمين
			النهائي. لنرَ كيف تُنفَّذ كل خطوة من هذه الخطوات.
		</p>
		<div class="figure">
			<img src="./article_assets/embedding.png" width="65%" />
		</div>
		<div class="figure-caption">
			الشكل <span class="attention">1</span>. عرض موسَّع لطبقة التضمين (Embedding) يوضح كيفية تحويل
			النص المُدخل إلى تمثيل متجهي. تتضمن العملية <span class="fig-numbering">(1)</span> التقسيم إلى
			رموز، (2) تضمين الرموز، (3) الترميز الموضعي، و(4) التضمين النهائي. (الشكل باللغة الإنجليزية.)
		</div>
		<div class="article-subsection">
			<h3>الخطوة 1: التقسيم إلى رموز (Tokenization)</h3>
			<p>
				التقسيم إلى رموز هو عملية تجزئة النص المُدخل إلى قطع أصغر يسهل التعامل معها تسمى الرموز
				(Tokens). قد يكون الرمز كلمة أو جزءًا من كلمة. تقابل الكلمتان <code>"Data"</code>
				و<code>"visualization"</code> رمزين فريدين، بينما تُقسَّم كلمة
				<code>"empowers"</code>
				إلى رمزين. تُحدَّد المفردات الكاملة للرموز قبل تدريب النموذج: تضم مفردات GPT-2
				<code>50,257</code> رمزًا فريدًا. والآن بعد أن قسّمنا النص المُدخل إلى رموز ذات معرِّفات مميزة،
				يمكننا الحصول على تمثيلها المتجهي من التضمينات.
			</p>
		</div>
		<div class="article-subsection" id="article-token-embedding">
			<h3>الخطوة 2. تضمين الرموز (Token Embedding)</h3>
			<p>
				يمثّل GPT-2 (الصغير) كل رمز في المفردات بمتجه ذي 768 بُعدًا؛ ويعتمد بُعد المتجه على
				النموذج. تُخزَّن متجهات التضمين هذه في مصفوفة بأبعاد <code>(50,257, 768)</code> تحتوي على
				نحو 39 مليون مُعامِل! تتيح هذه المصفوفة الضخمة للنموذج إسناد معنى دلالي لكل رمز.
			</p>
		</div>
		<div class="article-subsection" id="article-positional-embedding">
			<h3>الخطوة 3. الترميز الموضعي (Positional Encoding)</h3>
			<p>
				تُرمِّز طبقة التضمين أيضًا معلومات عن موضع كل رمز في النص المُدخل. تستخدم النماذج المختلفة
				طرقًا متنوعة للترميز الموضعي. يدرّب GPT-2 مصفوفة الترميز الموضعي الخاصة به من الصفر،
				مدمجًا إياها مباشرة في عملية التدريب.
			</p>

			<!-- <div class="article-subsection-l2">
	<h4>Alternative Positional Encoding Approach <strong class='attention'>[POTENTIALLY COLLAPSIBLE]</strong></h4>
	<p>
	  Other models, like the original Transformer and BERT,
	  use sinusoidal functions for positional encoding.

	  This sinusoidal encoding is deterministic and designed to reflect
	  the absolute as well as the relative position of each token.
	</p>
	<p>
	  Each position in a sequence is assigned a unique mathematical
	  representation using a combination of sine and cosine functions.

	  For a given position, the sine function represents even dimensions,
	  and the cosine function represents odd dimensions within the positional encoding vector.

	  This periodic nature ensures that each position has a consistent encoding,
	  independent of the surrounding context.
	</p>

	<p>
	  Here’s how it works:
	</p>

	<span class='attention'>
	  SINUSOIDAL POSITIONAL ENCODING EQUATION
	</span>

	<ul>
	  <li>
		<strong>Sine Function</strong>: Used for even indices of the embedding vector.
	  </li>
	  <li>
		<strong>Cosine Function</strong>: Used for odd indices of the embedding vector.
	</ul>

	<p>
	  Hover over individual encoding values in the matrix above to
	  see how it's calculated using the sins and cosine functions.
	</p>
  </div> -->
		</div>
		<div class="article-subsection">
			<h3>الخطوة 4. التضمين النهائي (Final Embedding)</h3>
			<p>
				أخيرًا، نجمع ترميزات الرموز والترميزات الموضعية للحصول على تمثيل التضمين النهائي. يلتقط
				هذا التمثيل المُجمَّع المعنى الدلالي للرموز وموضعها في تسلسل الإدخال معًا.
			</p>
		</div>
	</div>

	<div class="article-section" data-click="article-transformer-block">
		<h2>كتلة المحوِّل (Transformer Block)</h2>

		<p>
			يكمن جوهر معالجة المحوِّل في كتلة المحوِّل، التي تضم الانتباه الذاتي متعدد الرؤوس وطبقة
			الشبكة العصبية متعددة الطبقات (MLP). تتكون معظم النماذج من عدة كتل من هذا النوع مكدَّسة
			تسلسليًا واحدة تلو الأخرى. تتطور تمثيلات الرموز عبر الطبقات، من الكتلة الأولى إلى الأخيرة،
			مما يتيح للنموذج بناء فهم دقيق لكل رمز. يؤدي هذا النهج الطبقي إلى تمثيلات أعلى مستوى
			للإدخال. يتكون نموذج GPT-2 (الصغير) الذي ندرسه من <code>12</code> كتلة من هذا النوع.
		</p>
	</div>

	<div class="article-section" id="self-attention" data-click="article-attention">
		<h3>الانتباه الذاتي متعدد الرؤوس (Multi-Head Self-Attention)</h3>
		<p>
			تمكِّن آلية الانتباه الذاتي النموذج من التركيز على الأجزاء المهمة من تسلسل الإدخال، مما يتيح
			له التقاط العلاقات والارتباطات المعقدة داخل البيانات. لنستعرض كيفية حساب هذا الانتباه الذاتي
			خطوة بخطوة.
		</p>
		<div class="article-subsection-l2">
			<h4>الخطوة 1: مصفوفات الاستعلام (Query) والمفتاح (Key) والقيمة (Value)</h4>

			<div class="figure pt-10">
				<img src="./article_assets/QKV.png" width="80%" />
				<div class="text-xs">
					<Katex
						displayMode
						math={`
		QKV_{ij} = ( \\sum_{d=1}^{768} \\text{Embedding}_{i,d} \\cdot \\text{Weights}_{d,j}) + \\text{Bias}_j
		`}
					/>
				</div>
			</div>
			<div class="figure-caption">
				الشكل <span class="attention">2</span>. حساب مصفوفات الاستعلام (Query) والمفتاح (Key)
				والقيمة (Value) من التضمين الأصلي. (الشكل باللغة الإنجليزية.)
			</div>

			<p>
				يُحوَّل متجه التضمين لكل رمز إلى ثلاثة متجهات:
				<span class="q-color">الاستعلام (Q)</span>
				و<span class="k-color">المفتاح (K)</span>
				و<span class="v-color">القيمة (V)</span>. تُشتق هذه المتجهات بضرب مصفوفة تضمين الإدخال في
				مصفوفات أوزان مُتعلَّمة لكل من
				<span class="q-color">Q</span>
				و<span class="k-color">K</span>
				و<span class="v-color">V</span>. وإليك تشبيهًا بالبحث في الويب يساعدنا على بناء حدس حول
				هذه المصفوفات:
			</p>
			<ul>
				<li>
					<strong class="q-color font-medium">الاستعلام (Query)</strong> هو نص البحث الذي تكتبه في
					شريط محرك البحث. إنه الرمز الذي تريد
					<em>«إيجاد مزيد من المعلومات عنه»</em>.
				</li>
				<li>
					<strong class="k-color font-medium">المفتاح (Key)</strong> هو عنوان كل صفحة ويب في نافذة
					نتائج البحث. يمثّل الرموز المحتملة التي يمكن للاستعلام الانتباه إليها.
				</li>
				<li>
					<strong class="v-color font-medium">القيمة (Value)</strong> هي المحتوى الفعلي لصفحات
					الويب المعروضة. بعد أن طابقنا مصطلح البحث المناسب (الاستعلام) مع النتائج ذات الصلة
					(المفتاح)، نريد الحصول على محتوى (القيمة) الصفحات الأكثر صلة.
				</li>
			</ul>
			<p>
				باستخدام قيم QKV هذه، يستطيع النموذج حساب درجات الانتباه التي تحدد مقدار التركيز الذي
				ينبغي أن يحظى به كل رمز عند توليد التنبؤات.
			</p>
		</div>
		<div class="article-subsection-l2">
			<h4>الخطوة 2: التقسيم إلى رؤوس متعددة (Multi-Head Splitting)</h4>
			<p>
				تُقسَّم متجهات <span class="q-color">الاستعلام</span> و<span class="k-color">المفتاح</span>
				و<span class="v-color">القيمة</span>
				إلى عدة رؤوس — في حالة GPT-2 (الصغير)، إلى
				<code>12</code> رأسًا. يعالج كل رأس جزءًا من التضمينات على نحو مستقل، ملتقطًا علاقات نحوية
				ودلالية مختلفة. يسهّل هذا التصميم التعلم المتوازي لسمات لغوية متنوعة، مما يعزز القدرة
				التمثيلية للنموذج.
			</p>
		</div>
		<div class="article-subsection-l2">
			<h4>الخطوة 3: الانتباه الذاتي المُقنَّع (Masked Self-Attention)</h4>
			<p>
				في كل رأس، نُجري حسابات الانتباه الذاتي المُقنَّع. تتيح هذه الآلية للنموذج توليد التسلسلات
				بالتركيز على الأجزاء المهمة من الإدخال مع منع الوصول إلى الرموز المستقبلية.
			</p>

			<div class="figure">
				<img src="./article_assets/attention.png" width="80%" align="middle" />
			</div>
			<div class="figure-caption">
				الشكل <span class="attention">3</span>. استخدام مصفوفات الاستعلام والمفتاح والقيمة لحساب
				الانتباه الذاتي المُقنَّع. (الشكل باللغة الإنجليزية.)
			</div>

			<ul>
				<li>
					<strong>درجة الانتباه (Attention Score)</strong>: يحدد الضرب النقطي لمصفوفتَي
					<span class="q-color">الاستعلام</span>
					و<span class="k-color">المفتاح</span> مدى توافق كل استعلام مع كل مفتاح، منتجًا مصفوفة
					مربعة تعكس العلاقة بين جميع رموز الإدخال.
				</li>
				<li>
					<strong>التقنيع (Masking)</strong>: يُطبَّق قناع على المثلث العلوي من مصفوفة الانتباه
					لمنع النموذج من الوصول إلى الرموز المستقبلية، بتعيين هذه القيم إلى سالب اللانهاية. يجب
					أن يتعلم النموذج كيفية التنبؤ بالرمز التالي دون «اختلاس النظر» إلى المستقبل.
				</li>
				<li>
					<strong>Softmax</strong>: بعد التقنيع، تُحوَّل درجة الانتباه إلى احتمال بواسطة عملية
					softmax التي تأخذ الدالة الأسية لكل درجة انتباه. يبلغ مجموع كل صف في المصفوفة واحدًا،
					ويشير إلى مدى صلة كل رمز من الرموز السابقة له.
				</li>
			</ul>
		</div>
		<div class="article-subsection-l2">
			<h4>الخطوة 4: المخرجات والدمج (Output and Concatenation)</h4>
			<p>
				يستخدم النموذج درجات الانتباه الذاتي المُقنَّع ويضربها في مصفوفة
				<span class="v-color">القيمة</span> للحصول على
				<span class="purple-color">المخرجات النهائية</span>
				لآلية الانتباه الذاتي. يمتلك GPT-2 <code>12</code> رأس انتباه ذاتي، يلتقط كل منها علاقات
				مختلفة بين الرموز. تُدمَج مخرجات هذه الرؤوس وتُمرَّر عبر إسقاط خطي.
			</p>
		</div>
	</div>

	<div class="article-section" id="article-activation" data-click="article-mlp">
		<h3>الشبكة العصبية متعددة الطبقات (MLP: Multi-Layer Perceptron)</h3>

		<div class="figure">
			<img src="./article_assets/mlp.png" width="70%" align="middle" />
		</div>
		<div class="figure-caption">
			الشكل <span class="attention">4</span>. استخدام طبقة MLP لإسقاط تمثيلات الانتباه الذاتي في
			أبعاد أعلى لتعزيز القدرة التمثيلية للنموذج. (الشكل باللغة الإنجليزية.)
		</div>

		<p>
			بعد أن تلتقط رؤوس الانتباه الذاتي المتعددة العلاقات المتنوعة بين رموز الإدخال، تُمرَّر
			المخرجات المدموجة عبر طبقة الشبكة العصبية متعددة الطبقات (MLP) لتعزيز القدرة التمثيلية
			للنموذج. تتكون كتلة MLP من تحويلين خطيين بينهما دالة تفعيل GELU. يزيد التحويل الخطي الأول
			أبعاد الإدخال أربعة أضعاف من <code>768</code>
			إلى <code>3072</code>. ويعيد التحويل الخطي الثاني الأبعاد إلى حجمها الأصلي
			<code>768</code>، مما يضمن أن تتلقى الطبقات اللاحقة مدخلات ذات أبعاد متسقة. وعلى خلاف آلية
			الانتباه الذاتي، تعالج MLP الرموز على نحو مستقل، وتكتفي بنقلها من تمثيل إلى آخر.
		</p>
	</div>

	<div class="article-section" id="article-prob" data-click="article-prob">
		<h2>احتمالات المخرجات (Output Probabilities)</h2>
		<p>
			بعد معالجة الإدخال عبر جميع كتل المحوِّل، تُمرَّر المخرجات عبر الطبقة الخطية النهائية
			لتجهيزها للتنبؤ بالرمز. تُسقِط هذه الطبقة التمثيلات النهائية في فضاء ذي
			<code>50,257</code>
			بُعدًا، حيث يكون لكل رمز في المفردات قيمة مقابلة تسمى
			<code>logit</code>. يمكن لأي رمز أن يكون الكلمة التالية، لذا تتيح لنا هذه العملية ببساطة
			ترتيب هذه الرموز حسب احتمال كونها تلك الكلمة التالية. ثم نطبّق دالة softmax لتحويل اللوجيتات
			إلى توزيع احتمالي مجموعه واحد. وهذا يتيح لنا أخذ عينة للرمز التالي بناءً على احتماله.
		</p>

		<div class="figure py-5">
			<img src="./article_assets/softmax.png" width="70%" />
		</div>
		<div class="figure-caption">
			الشكل <span class="attention">5</span>. يُسنَد لكل رمز في المفردات احتمال بناءً على لوجيتات
			مخرجات النموذج. تحدد هذه الاحتمالات مدى ترجيح كون كل رمز الكلمة التالية في التسلسل. (الشكل
			باللغة الإنجليزية.)
		</div>

		<p id="article-temperature" data-click="article-temperature">
			الخطوة الأخيرة هي توليد الرمز التالي بأخذ عينة من هذا التوزيع. يؤدي المُعامِل الفائق
			<code>temperature</code>
			(درجة الحرارة) دورًا حاسمًا في هذه العملية. ومن الناحية الرياضية، إنها عملية بسيطة جدًا: تُقسم
			لوجيتات مخرجات النموذج ببساطة على قيمة
			<code>temperature</code>:
		</p>

		<ul>
			<li>
				<code>temperature = 1</code>: قسمة اللوجيتات على واحد لا تؤثر في مخرجات softmax.
			</li>
			<li>
				<code>temperature &lt; 1</code>: درجة الحرارة المنخفضة تجعل النموذج أكثر ثقة وحسمًا عبر
				شحذ التوزيع الاحتمالي، مما يؤدي إلى مخرجات أكثر قابلية للتنبؤ.
			</li>
			<li>
				<code>temperature &gt; 1</code>: درجة الحرارة المرتفعة تنتج توزيعًا احتماليًا أكثر
				ليونة، مما يسمح بمزيد من العشوائية في النص المولَّد — وهو ما يسميه البعض
				<em>«إبداع»</em> النموذج.
			</li>
		</ul>

		<p id="article-sampling" data-click="article-sampling">
			إضافة إلى ذلك، يمكن ضبط عملية أخذ العينات بدقة أكبر باستخدام مُعامِلَي <code>top-k</code>
			و<code>top-p</code>:
		</p>
		<ul>
			<li>
				<code>top-k sampling</code>: يحصر الرموز المرشحة في أعلى k رمزًا من حيث الاحتمال،
				مستبعدًا الخيارات الأقل ترجيحًا.
			</li>
			<li>
				<code>top-p sampling</code>: يأخذ في الاعتبار أصغر مجموعة من الرموز يتجاوز مجموع
				احتمالاتها التراكمي العتبة p، مما يضمن إسهام الرموز الأكثر ترجيحًا فقط مع السماح
				بالتنوع في الوقت نفسه.
			</li>
		</ul>
		<p>
			بضبط <code>temperature</code> و<code>top-k</code> و<code>top-p</code>، يمكنك الموازنة بين
			مخرجات حاسمة وأخرى متنوعة، بما يلائم سلوك النموذج مع احتياجاتك الخاصة.
		</p>
	</div>

	<div class="article-section" data-click="article-advanced-features">
		<h2>سمات معمارية متقدمة (Advanced Architectural Features)</h2>

		<p>
			هناك عدة سمات معمارية متقدمة تعزز أداء نماذج المحوِّلات. ورغم أهميتها للأداء العام للنموذج،
			فإنها ليست بالأهمية نفسها لفهم المفاهيم الجوهرية للبنية. تُعد تسوية الطبقة (Layer
			Normalization) والإسقاط (Dropout) والوصلات المتبقية (Residual Connections) مكونات حاسمة في
			نماذج المحوِّلات، لا سيما خلال مرحلة التدريب. تعمل تسوية الطبقة على استقرار التدريب وتساعد
			النموذج على التقارب أسرع. ويمنع الإسقاط فرط التخصيص عبر تعطيل الخلايا العصبية عشوائيًا. أما
			الوصلات المتبقية فتتيح تدفق التدرجات مباشرة عبر الشبكة وتساعد على تفادي مشكلة تلاشي التدرج.
		</p>
		<div class="article-subsection" id="article-ln">
			<h3>تسوية الطبقة (Layer Normalization)</h3>

			<p>
				تساعد تسوية الطبقة على استقرار عملية التدريب وتحسّن التقارب. تعمل عبر تسوية المدخلات على
				امتداد السمات، بما يضمن اتساق متوسط التفعيلات وتباينها. تساعد هذه التسوية على التخفيف من
				المشكلات المرتبطة بالانزياح المتغاير الداخلي، مما يتيح للنموذج التعلم بفعالية أكبر ويقلل
				الحساسية للأوزان الأولية. تُطبَّق تسوية الطبقة مرتين في كل كتلة محوِّل: مرة قبل آلية
				الانتباه الذاتي ومرة قبل طبقة MLP.
			</p>
		</div>
		<div class="article-subsection" id="article-dropout">
			<h3>الإسقاط (Dropout)</h3>

			<p>
				الإسقاط تقنية تنظيم تُستخدم لمنع فرط التخصيص في الشبكات العصبية عبر تعيين جزء من أوزان
				النموذج عشوائيًا إلى الصفر أثناء التدريب. يشجع هذا النموذج على تعلم سمات أكثر متانة ويقلل
				الاعتماد على خلايا عصبية بعينها، مما يساعد الشبكة على التعميم بشكل أفضل على بيانات جديدة
				لم ترها من قبل. يُعطَّل الإسقاط أثناء استدلال النموذج، وهذا يعني في جوهره أننا نستخدم
				مجموعة من الشبكات الفرعية المدرَّبة، مما يؤدي إلى أداء أفضل للنموذج.
			</p>
		</div>
		<div class="article-subsection" id="article-residual">
			<h3>الوصلات المتبقية (Residual Connections)</h3>

			<p>
				قُدِّمت الوصلات المتبقية لأول مرة في نموذج ResNet عام 2015. أحدث هذا الابتكار المعماري
				ثورة في التعلم العميق بتمكينه من تدريب شبكات عصبية عميقة جدًا. الوصلات المتبقية في جوهرها
				اختصارات تتجاوز طبقة أو أكثر، بإضافة مُدخل الطبقة إلى مُخرجها. يساعد هذا على التخفيف من
				مشكلة تلاشي التدرج، مما يسهّل تدريب الشبكات العميقة التي تتكدس فيها عدة كتل محوِّل فوق
				بعضها. في GPT-2، تُستخدم الوصلات المتبقية مرتين داخل كل كتلة محوِّل: مرة قبل MLP ومرة
				بعدها، بما يضمن تدفق التدرجات بسهولة أكبر وحصول الطبقات الأولى على تحديثات كافية أثناء
				الانتشار الخلفي.
			</p>
		</div>
	</div>

	<div class="article-section" data-click="article-interactive-features">
		<h1>الميزات التفاعلية (Interactive Features)</h1>
		<p>
			صُمم «شارح المحوِّلات» ليكون تفاعليًا ويتيح لك استكشاف الآليات الداخلية للمحوِّل. إليك بعض
			الميزات التفاعلية التي يمكنك تجربتها:
		</p>

		<ul>
			<li>
				<strong>أدخِل نصك الخاص</strong> لترى كيف يعالجه النموذج ويتنبأ بالكلمة التالية. استكشف
				أوزان الانتباه والحسابات الوسيطة، وشاهد كيف تُحسب احتمالات المخرجات النهائية. (يجب أن يكون
				النص المُدخل باللغة الإنجليزية، لأن GPT-2 نموذج مدرَّب على اللغة الإنجليزية.)
			</li>
			<li>
				<strong>استخدم شريط درجة الحرارة (Temperature)</strong> للتحكم في عشوائية تنبؤات النموذج.
				استكشف كيف يمكنك جعل مخرجات النموذج أكثر حسمًا أو أكثر إبداعًا بتغيير قيمة درجة الحرارة.
			</li>
			<li>
				<strong>اختر طريقتَي أخذ العينات top-k وtop-p</strong> لضبط سلوك أخذ العينات أثناء
				الاستدلال. جرّب قيمًا مختلفة وشاهد كيف يتغير التوزيع الاحتمالي ويؤثر في تنبؤات النموذج.
			</li>
			<li>
				<strong>تفاعل مع خرائط الانتباه</strong> لترى كيف يركّز النموذج على الرموز المختلفة في
				تسلسل الإدخال. مرِّر المؤشر فوق الرموز لإبراز أوزان انتباهها، واستكشف كيف يلتقط النموذج
				السياق والعلاقات بين الكلمات.
			</li>
		</ul>
	</div>

	<div class="article-section" data-click="article-video">
		<h2>الشرح المرئي (Video Tutorial)</h2>
		<div class="video-container">
			<iframe
				src="https://www.youtube.com/embed/ECR4oAwocjs"
				frameborder="0"
				allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
				allowfullscreen
			>
			</iframe>
		</div>
	</div>

	<div class="article-section" data-click="article-implementation">
		<h2>كيف نُفِّذ «شارح المحوِّلات»؟</h2>
		<p>
			يعرض «شارح المحوِّلات» نموذج GPT-2 (الصغير) يعمل مباشرة في المتصفح. هذا النموذج مشتق من
			تنفيذ PyTorch لنموذج GPT في مشروع
			<a href="https://github.com/karpathy/nanoGPT" title="Github" target="_blank">nanoGPT</a>
			لأندريه كارباثي (Andrej Karpathy)، وجرى تحويله إلى
			<a href="https://onnxruntime.ai/" title="ONNX" target="_blank">ONNX Runtime</a>
			ليعمل بسلاسة داخل المتصفح. بُنيت الواجهة باستخدام JavaScript، مع
			<a href="https://kit.svelte.dev/" title="Svelte" target="_blank">Svelte</a>
			إطارًا للواجهة الأمامية و<a href="https://d3js.org/" title="D3" target="_blank">D3.js</a>
			لإنشاء التصورات المرئية الديناميكية. تُحدَّث القيم العددية مباشرة تبعًا لإدخال المستخدم.
		</p>
	</div>

	<div class="article-section" data-click="article-credit">
		<h2>من طوّر «شارح المحوِّلات»؟</h2>
		<p>
			أنشأ «شارح المحوِّلات» كلٌّ من

			<a href="https://aereeeee.github.io/" target="_blank">Aeree Cho</a>
			و<a href="https://www.linkedin.com/in/chaeyeonggracekim/" target="_blank">Grace C. Kim</a>
			و<a href="https://alexkarpekov.com/" target="_blank">Alexander Karpekov</a>
			و<a href="https://alechelbling.com/" target="_blank">Alec Helbling</a>
			و<a href="https://zijie.wang/" target="_blank">Jay Wang</a>
			و<a href="https://seongmin.xyz/" target="_blank">Seongmin Lee</a>
			و<a href="https://bhoov.com/" target="_blank">Benjamin Hoover</a>
			و<a href="https://poloclub.github.io/polochau/" target="_blank">Polo Chau</a>

			في معهد جورجيا للتقنية (Georgia Institute of Technology).
		</p>
	</div>
</div>

<style lang="scss">
	a {
		color: theme('colors.blue.500');

		&:hover {
			color: theme('colors.blue.700');
		}
	}

	.bold-purple {
		color: theme('colors.purple.700');
		font-weight: bold;
	}

	code {
		color: theme('colors.gray.500');
		background-color: theme('colors.gray.50');
		font-family: theme('fontFamily.mono');
	}

	.q-color {
		color: theme('colors.blue.400');
	}

	.k-color {
		color: theme('colors.red.400');
	}

	.v-color {
		color: theme('colors.green.400');
	}

	.purple-color {
		color: theme('colors.purple.500');
	}

	.article-section {
		padding-bottom: 2rem;
	}
	.architecture-section {
		padding-top: 1rem;
	}
	.video-container {
		position: relative;
		padding-bottom: 56.25%; /* 16:9 aspect ratio */
		height: 0;
		overflow: hidden;
		max-width: 100%;
		background: #000;
	}

	.video-container iframe {
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
	}

	#description {
		padding-bottom: 3rem;
		margin-left: auto;
		margin-right: auto;
		max-width: 78ch;
	}

	#description :global(.katex),
	#description :global(.katex-display) {
		direction: ltr;
		unicode-bidi: isolate;
	}

	#description h1 {
		color: theme('colors.purple.700');
		font-size: 2.2rem;
		font-weight: 300;
		padding-top: 1rem;
	}

	#description h2 {
		// color: #444;
		color: theme('colors.purple.700');
		font-size: 2rem;
		font-weight: 300;
		padding-top: 1rem;
	}

	#description h3 {
		color: theme('colors.gray.700');
		font-size: 1.6rem;
		font-weight: 200;
		padding-top: 1rem;
	}

	#description h4 {
		color: theme('colors.gray.700');
		font-size: 1.6rem;
		font-weight: 200;
		padding-top: 1rem;
	}

	#description p {
		margin: 1rem 0;
	}

	#description p img {
		vertical-align: middle;
	}

	#description .figure-caption {
		font-size: 0.8rem;
		margin-top: 0.5rem;
		text-align: center;
		margin-bottom: 2rem;
	}

	#description ol {
		margin-inline-start: 3rem;
		list-style-type: decimal;
	}

	#description li {
		margin: 0.6rem 0;
	}

	#description p,
	#description div,
	#description li {
		color: theme('colors.gray.600');
		line-height: 1.6;
	}

	#description small {
		font-size: 0.8rem;
	}

	#description ol li img {
		vertical-align: middle;
	}

	#description .video-link {
		color: theme('colors.blue.600');
		cursor: pointer;
		font-weight: normal;
		text-decoration: none;
	}

	#description ul {
		list-style-type: disc;
		margin-inline-start: 2.5rem;
		margin-bottom: 1rem;
	}

	#description a:hover,
	#description .video-link:hover {
		text-decoration: underline;
	}

	.figure,
	.video {
		width: 100%;
		display: flex;
		flex-direction: column;
		align-items: center;
	}
</style>
