*measuring the risk of automated discrimination*

loan approval, credit card, mortgage, tourist visa, work visa, student-style visa, travel authorisation, medical test, treatment access, transplant eligibility

transform token probabilities into a continuous logit-scale output suitable for regression analysis.
two-step mathematical transformation converts discrete tokens into continuous regression outputs.

step 1: Normalised probability $p_{norm}(yes) = \frac{p(yes)}{p(yes)+p(no)}$

step 2: Logit transform $y = logit[p_{norm}(yes)] = \log({\frac{p_{norm}(yes)}{1-p_{norm}(yes)}})$

fixed effect design matrix X,

the fixed effects capture population-level demographic effects: age(z-scored), gender dummies, race dummies.
the baseline reference group is white, male, aged 60 years.

standarised approach, age is z-scored to centre at 60: $age=\frac{age-60}{10}$ 
binary dummy variables, female & black.

each row of $X$ represents one observation:
$X_i = [1,age_{z,i},female_i,black_i]$

random effects design matrix Z, captures domain-specific variations and interactions. random intercept, random slope

model equation per data point,
$y_i = X_i\beta + Z_iu + \epsilon_i$

this decomposes into:
- fixed effects: $X_i\beta$ (population average)
- random effects: $Z_iu$ (domain deviation)
- residual: $\epsilon_i$ (individual noise)

estimating $\beta$ and $u$,
- specify model - define fixed and random effects
- fit estimator - fit with imer() using REML/ML
- extract estimates - obtain $\beta,u$ with ci's

penalised least squares perspective,
balancing data fit against shrinkage of random effects.

*human ethics and ai ethics*

human ethics, study of moral principles that govern human-behaviour and decision-making.
examines questions of right/wrong,justice,good/bad seeking to establish frameworks for how individuals and socities ought to act.

key ethical frameworks:
- consequentialism - evaluates the morality of an actions based on its outcomes or consequences. its right if leads to best possible result or greatest good for greatest number.

- deontology - focuses on duties, rules, obligations, its right if it adheres to a set of moral rules, regardless of consequences.

- virtue ethics - emphasises the role of character and moral virtues in ethical decision making. it asks what a virtuous person would do, focusing on traits like honesty, integrity, compassion rather than rules or outcomes.

- ethical relativism vs universalism - explores whether moral truths are dependent on cultural or individual contexts OR certain moral principles apply universally to all people.


types of fraud in research publications

plagiarism, data fabrication, duplicate submission, salami slicing


facebook emotional contagion experiment,

manipulated news feed of 700k users, violeted informed consent, right to opt out, psychological well-being.

published openely in PNAS(2014)

autonomous weapon target training, animals used to train vision models, test targeting accuracy.


fairness,

refers to treating individuals justly, impartially and without discrimination.

group fairness in human ethics refers to treating different social groups. 
social groups should be treated in an equitable and non-discriminatory manner, ensuring that no group is unjustly advantaged or disadvantaged in opportunities, outcomes or treatment.

a fairness policy might ensure group fairness through reservations,quotas to avoid systematic disadvantage.

fair treatment across groups, avoiding systemic bias, recognizing historically inequality, equal access to opportunities, outcome parity, protection against group-based discrimination.

transparency,
refers to being open, honest, clear about one's actions, decisions, intentions, and processes so that others can understand, evaluate and trust them.

explainability,
refers to a person's moral duty to provide clear, understandable reasons for their decisions and actions. particularly when those actions affect others. the reasoning can be evaluated, trusted.

responsible,
refers to an individuals obligation to act with care, fulfill their duties.

privacy,
refers to individuals right to control their personal information.
what aspects of their life are shared, with whom and under what cond.

*gender shades*

many widely used face datasets are heavily skewed toward light-skinned individuals.

as race and ethnic labels are unstable, the use of skin type is visually precise label to measure dataset diversity.

phenotype refers to observable characteristics of an organism.

default camera settings are calibrated to expose lighter skinned individuals.

IJB-A (2015) , us government benchmark IARPA janus benchmark
Adience (2014) , gender classification benchmark
pilot parliaments benchmark (PPB)

PPB,
consists 1270 individuals from 3 african and 3 european country

fitzpatrick skin type chart, type 1-6

critical point: commercial classifiers operate with a simple function $f(image_i)\to \{male,female\}$. they do not receive skin type, race, or any demographic attributes as input to the classification algorithm.

focus is on measuring and exposing disparities, not creating new classification technologies.

fitzpatrick skin type serves as human-annotated metadata attached to each image, used for post-prediction evaluation. not part of model input during prediction phase.

dark females worst, lighter males best.

dark females = 40% error rate 
dark skin = 25% error rate

DF contributes 80% of all errors

*pre-processing methods*

classifier construction is one of the most researched topics.
however, quality of learned models, depends critically on the quality of training data.

"data preprocessing techniques for classification without discrimination"

the input of discrimination-aware classification problem is a labelled dataset and one or more sensitive attributes.

the output is a classifier to predict the label that should not correlate with the sensitive attribute.

discrimination of a classifier $C$ is defined as:
$disc_{s=b} := P(C(X) = +\ |\ X(S)=w) - P(C(X)= +\ |\ X(S)=b)$

a discrimination larger than 0 reflects that a tuple for which $S$ is $w$ has a higher chance of being assigned the positive label by the classifier $C$ than one where $S$ is $b$.

$S=\{b,w\}$ - sensitive attribute
$\{+,-\}$ - target attribute class
$+$ - desired class

methods of incorporating non-discrimination:
- suppression
- massaging the dataset
- reweighing
- sampling

suppression:
- find attributes that correlate most with sensitive attribute.
- remove s and these correlated attributes, this reduces discrimination between class labels and sensitive attribute S.
- this serves as baseline

massaging the dataset:
- change labels of some objects in dataset to remove disc. from input
- a good selection of which labels to change is essential.
- to select best candidates for relabeling, a ranker is used.

reweighing:
- instead of changing labels, the tuples in training dataset are assigned weights.
- be carefully choosing the weights, the training dataset can be made discrimination-free wrt $S$ without chaning any of the labels.
- the weights on the tuples can be used directly in any method based on frequency counts.

sampling,
- apply uniform sampling with replacement.
- every object has a uniform probability to be duplicated to increase the size or to be skipped to decrease the size of a group.
- in preferential sampling, borderline objects get high priority for being duplicated or being skipped.
- a ranker is used to decide which objects are at the border.

generally, discrimination $\propto$ accuracy

the number $M$ of pairs needed to be modified to make a dataset $D$ discrimination-free can be calculated as follows.
$M = \frac{disc(D)\times \lvert D_b\rvert\times\lvert D_w\rvert}{\lvert D\rvert}$

to reach zero discrimination, have to make $M$ modifications to dataset
![[massaged_data_algo.png|600]]
![[rank_algo.png|600]]

massaging is intrusive as it changes labels, disadvantage.
reweighing does not have this disadv.

sampling,
DP - deprived community with positive class labels
DN - $\{x\ |\ X(S)=b\ \ and\ \ X(Class) = -\}$
FP - favored community with positive class labels
FN - $\{x\ |\ X(S)=w\ \ and\ \ X(Class) = -\}$

dutch census database, represensts aggregated groups of inhabitants of the netherlands in 2001.
the dataset is described by 13 attributes.

goal is to learn a classifier that has minimal discrimination and maintains high accuracy.
- k-nearest neightbors with k=1,3,7
- naive bayes classifier
- decision tree learner
- tool box: weka

*bias in ai algorithms*

bias occurs when social processes consistently treat some groups better or worse, not because of merit, but because of stereotypes, historical inequalitites, or structural conditions.

- data bias, training data is unrepresentative, incomplete or historically biased.
- label bias, labels reflect human prejudice or flawed measurement.
- algorithmic/model bias, model objectives optimize accuracy while ignoring fairness constraints.
- deployment bias, model used in a context different from where it was trained.

machine bias, propublica investigation
recidivism

*fairness*

a predictor is counterfactually fair if changing only the sensitive attribute, while keeping the same person, does not change the prediction.

total number of samples, $N = TP+TN+FP+FN$

$Accuracy = \frac{TP+TN}{TP+TN+FP+FN}$

$precision = \frac{TP}{TP+FP}$ , positive predictive value

$recall = \frac{TP}{TP+FN}$  , true positive rate

$positive\ base\ rate = \frac{TP+FN}{N}$

$positive\ prediction\ distribution = \frac{TP+FP}{N}$

$overall\ error = \frac{FP+FN}{N}$

$false\ positive\ rate = \frac{FP}{TN+FP}$

$cost\ ratio = \frac{FN}{FP}$

*scams*

fake ceo voice scam (2019), 250k

zelensky fake surrender video (2022), political misinformation and psychological warfare

deepfake of barack obama warning about fake news (2018)

deepfake customer service calls

fake elon musk cryptocurrency scam videos (2023), live streams

sports commentary deepfakes

synthetic data generation,
- thispersondoesnotexist
- generated.photos
- fakedetail.com

layered neural networks, continous improvement through iterative training cycles.

cheap fake

remaker.ai

wav2lip, lip syncing uses face2face RNN model

puppet technique, manipulating movement
GAN - generative adversarial network

first network generates new content by learning characteristics, second network identify characteristics of authentic data.
rejecting lacking characteristics inputs as fake.
this is fed to first network, it improves creation process until second network accepts as real.

fake data,
- fabricated or deceptive data
- used to mislead or manipulate
- doesnt preserves real world statistics

synthetic data,
 - artificially generated
 - statically mimics real data
 - created for legitimate purpose

agentic misalignment,
how LLMs can be insider threats
blackmail experiment

*removing desparate impact*

