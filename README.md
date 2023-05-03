Download Link: https://assignmentchef.com/product/solved-cs-208-hw-1-reidentification-reconstruction-and-membership-attacks
<br>
<h1>1.    Reidentification Attack</h1>

In the GitHub repo,<a href="#_ftn1" name="_ftnref1"><sup>[1]</sup></a> you will find the Public Use Micro Sample (PUMS) dataset from the 2000 US Census FultonPUMS5full.csv. This is a sample from the “Long Form” from Georgia residents, which contained many more questions than the regular questionnaire, and was randomly assigned to some individuals during the decennial Census. (It has since been replaced by a continuously collected survey known as the <em>American Community Survey</em>.)

Also in that folder is the codebook file for the PUMS dataset that lists the variables available in the release. Note this is the 5% sample which means that five percent of records are randomly sampled and released.

In the style of Latanya Sweeney’s record linkage reidentification attack,<a href="#_ftn2" name="_ftnref2"><sup>[2]</sup></a> propose a reidentification attack on the PUMS dataset by identifying demographic variables that, if known from another auxiliary source, could uniquely identify individuals. Note that while Sweeney used zipcodes as the geographic indicator, individuals in this Census release are identified by Public Use Microdata Areas (PUMAs) which are Census constructed geographic areas that contain at least 100,000 individuals. State the variables you would use, and provide an approximate back-ofthe-envelope calculation of the number of individuals who would be unique in that combination of variables in a PUMA region.

<h1>2.    Reconstruction Attack</h1>

Among the variables in the 2000 PUMS dataset above is USCITIZEN, which asks the resident about their US Citizenship status. This is a sensitive piece of information, and including this question on the regular Census questionnaire has been a topic of recent controversy.<a href="#_ftn3" name="_ftnref3"><sup>[3]</sup></a> This PUMS dataset is public, but makes a good stand-in for a database that might be secured behind a query interface. We’ve provided a sample of size <em>n </em>= 100.

In this problem, you will run experiments to evaluate the performance of the reconstruction attack on determining individuals’ citizenship status. Treat the following variables in the dataset as public (so as an attacker you know them for all of the individuals in the dataset):

PUB =(SEX<em>,</em>AGE<em>,</em>EDUC<em>,</em>AGE<em>,</em>MARRIED<em>,</em>DIVORCED<em>,</em>LATINO<em>,</em>BLACK<em>,</em>ASIAN<em>,</em>CHILDREN<em>, </em>EMPLOYED<em>,</em>MILITARYSERVICE<em>,</em>DISABILITY<em>,</em>ENGLISHABILITY)<em>.</em>

<em> </em>

























Each query in your attack should specify a boolean predicate <em>p</em>(PUB) ∈ {0<em>,</em>1} on the public variables (e.g. <em>p</em>(AGE<em>/</em>EDUC <em>&gt; </em>4 &amp;&amp; SEX == 0)), and receive as an answer an approximation to the value:

X

USCITIZEN<em><sub>i</sub>,</em>

<em>i</em>:<em>p</em>(PUB<em><sub>i</sub></em>)=1

where <em>i </em>ranges over the <em>n </em>= 100 individuals in the PUMS dataset sample, FultonPUMS5sample100.csv, that we have provided.

Your attack should make 2<em>n </em>queries, where each query corresponds to a different predicate <em>p<sub>j</sub></em>, <em>j </em>= 1<em>,…,</em>2<em>n</em>.<a href="#_ftn4" name="_ftnref4"><sup>[4]</sup></a> Using the description of these predicates, the public data PUB<sub>1</sub><em>,…,</em>PUB<em><sub>n</sub></em>, and the noisy answers to the queries, you should try to reconstruct the USCITIZEN<em><sub>i </sub></em>bits for as many users as possible.

You will run experiments on how your attack performs against the following defenses:

<ul>

 <li>Rounding: round each result to the nearest multiple of <em>R </em>for a parameter <em>R</em></li>

 <li>Noise addition: add Gaussian noise of mean zero and variance <em>σ</em><sup>2</sup>, for a parameter <em>σ</em>, independently for each query.</li>

 <li>Subsampling: randomly subsample a set <em>T </em>consisting of <em>t </em>out of the <em>n </em>rows, for a paremeter <em>t</em>, and calculate the answer using only the rows in <em>T </em>(scaling up by a factor of <em>n/t</em>).</li>

</ul>

Varying parameters <em>R</em>, <em>σ</em>, and <em>T </em>as integers from 0 to <em>n</em>, produce plots showing and comparing the trade-off between the accuracy of the statistics (measured by root-mean-squared-error between answers and exact values) and the average fraction of values USCITIZEN<em><sub>i </sub></em>that are successfully reconstructed. For each parameter setting, run 10 experiments with fresh randomness and plot the average data points.

Make sure to identify the regime where your attack transitions from near-perfect reconstruction (fraction close to 1) to near-unsuccessful reconstruction (fraction close to 1/2). Add additional data points so that your graph is detailed around that transition point.

Note that you will be coding both the release mechanisms for each defense as well as the attack. The GitHub repo contains the code from the regression-based reconstruction attack from Monday’s class<a href="#_ftn5" name="_ftnref5"><sup>[5]</sup></a>. (Be sure to pull the most recent copy.) You can directly expand from this code if you are working in R, or use it as a template if you are working in Python.

<strong>BONUS: </strong>The above attack requires knowledge of all of the PUB<em><sub>i</sub></em>’s. Here we will sketch a version of the attack that only requires knowledge of a single PUB<em><sub>i </sub></em>and reconstructs USCITIZEN<em><sub>i </sub></em>for that particular individual. For extra credit, fill in the details and implement the attack and measure its performance.

Above, we suggested using a random hash function of the form <em>p</em>(<em>v</em>) = ((<sup>P</sup><em><sub>d </sub>r<sub>d</sub>v<sub>d</sub></em>) mod <em>P</em>) mod 2 to select subsets of the dataset. Instead, consider taking <em>P </em>to be a prime of magnitude larger than <em>n </em>by a small constant factor (somewhere between 2 and 10), choosing <em>r</em><sub>1</sub><em>,…,r<sub>d </sub></em>once and for all, and defining the hash function <em>h</em>(<em>v</em>) = <sup>P</sup><em><sub>d </sub>r<sub>d</sub>v<sub>d </sub></em>mod <em>P</em>. Since <em>P </em>is significantly larger than <em>n</em>, there will be few collisions of the PUB<em><sub>i</sub></em>’s under the hash function <em>h</em>. Now for each query <em>p<sub>j</sub></em>, pick a random number <em>s<sub>j </sub></em>∈ {0<em>,…,P </em>− 1}, and define <em>p<sub>j</sub></em>(<em>v</em>) = ((<em>s<sub>j </sub></em>· <em>h</em>(<em>v</em>)) mod <em>P</em>) mod 2. Now you can do your linear regression with <em>P </em>variables, one for each possible value of <em>h</em>(PUB), since <em>h </em>is not changing across the queries. To attack a particular individual <em>i</em>, we look at the result of the regression for the variable associated with <em>h</em>(PUB<em><sub>i</sub></em>). (If the regression is too slow, feel free to use smaller values of <em>n </em>and <em>P</em>)

<h1>3.    Membership Attack</h1>

Run a similar experiment to evaluate the effectiveness of the membership attack covered in class on the same sample of <em>n </em>= 100 from the PUMS dataset above. Specifically, find the highest level of accuracy (i.e. lowest RMSE) at which the expected fraction of bits that the reconstruction attack fails against all three defenses, where failure means reconstructing approximately 50% of the bits. Fix parameters for each of the three defenses that correspond to this level of accuracy, and produce a graph of the number of queries issued vs. the true positive probability of the membership attack (i.e. the probability that the attack says “IN” when Alice is a randomly chosen member of the dataset). You can use membershipAttackCompleted.r as a template, which contains the membership attack from lecture including all the modifications made during lecture. Here are guidelines for carrying out the attack:

<ul>

 <li>We can think of the binary values in the membership attack described in class either as actual attributes or the results of Boolean predicates applied to the attributes. Since there are not enough actual attributes in the PUMS dataset to run a membership attack, create derived attributes in the following way. For the <em>j</em>th “attribute” of user <em>i </em>in the membership attack, use the predicate <em>p<sub>j</sub></em>(PUB<em><sub>i</sub></em>), where <em>p<sub>j </sub></em>is a random predicate generated in the same way that you did in the reconstruction attack.</li>

 <li>Feel free use to use counts or means as your statistics, as they are equivalent up to a scaling by a factor of <em>n</em>. If you use means, be sure to scale the accuracy threshold you use accordingly.</li>

 <li>Increase the number of queries/attributes until either the true positive probabilities start to converge or it becomes computationally infeasible.</li>

 <li>Below we will mostly use notation from the membership attacks lecture, but we’ll use <em>m </em>for the number of queries (since above we used <em>d </em>for the number of attributes in PUB) and <em>ρ </em>= (<em>ρ</em><sub>1</sub><em>,…,ρ<sub>m</sub></em>) for the population probabilities (since above <em>p<sub>j </sub></em>denotes the <em>j</em>’th predicate).</li>

 <li>To calculate the vector <em>ρ </em>of population probabilities, you can either use the full Fulton Georgia PUMS dataset that we have provided (csv consisting of 25,766 individuals) or do an analytic calculation based on the random predicates you use.</li>

 <li>Set the false positive probability to be <em>δ </em>= 1<em>/</em>10<em>n</em>. To determine the corresponding threshold <em>T </em>= <em>T<sub>ρ,a</sub></em>, you can approximate the null distribution of your test statistic either using the resampling method shown in class on 2/11 or a normal approximation N(0<em>,σ</em><sup>2</sup>) where <em>σ</em><sup>2 </sup>is the variance of your test statistic. <a href="#_ftn6" name="_ftnref6"><sup>[6]</sup></a> Check that you are indeed achieving a small enough false positive probability by running your membership attack on some randomly chosen members of the full population dataset.</li>

</ul>

<h1>4.    Final Project Ideas</h1>

The final projects are a important focus of this course, and we want you to start thinking about yours as soon as possible. Please read the “Final Project Guidelines” (<a href="http://seas.harvard.edu/~salil/cs208/spring19/project-guidelines.pdf">http://seas.harvard. </a><a href="http://seas.harvard.edu/~salil/cs208/spring19/project-guidelines.pdf">edu/</a><a href="http://seas.harvard.edu/~salil/cs208/spring19/project-guidelines.pdf">~</a><a href="http://seas.harvard.edu/~salil/cs208/spring19/project-guidelines.pdf">salil/cs208/spring19/project-guidelines.pdf</a><a href="http://seas.harvard.edu/~salil/cs208/spring19/project-guidelines.pdf">)</a> document on the course website and submit about a paragraph as described in the “Topic Ideas” bullet.




<strong>Codebook for Census PUMS 5 Percent CS208 Datasets</strong>

<table width="617">

 <tbody>

  <tr>

   <td width="153">X.1</td>

   <td colspan="2" width="463">Deprecated, removed from dataset</td>

  </tr>

  <tr>

   <td width="153">state</td>

   <td colspan="2" width="463">The US State of residence.</td>

  </tr>

  <tr>

   <td width="153">puma</td>

   <td colspan="2" width="463">The Public Use Microdata Area, a Census constructed region of about 100,000 persons.</td>

  </tr>

  <tr>

   <td width="153">jpumarow</td>

   <td colspan="2" width="463">Deprecated, removed from dataset</td>

  </tr>

  <tr>

   <td width="153">serialno.household</td>

   <td colspan="2" width="463">Deprecated, removed from dataset</td>

  </tr>

  <tr>

   <td width="153">sex</td>

   <td colspan="2" width="463">0:        Male,1:        Female.</td>

  </tr>

  <tr>

   <td width="153">age</td>

   <td colspan="2" width="463">Age in years.</td>

  </tr>

  <tr>

   <td width="153">educ</td>

   <td colspan="2" width="463">1:        No schooling completed,2:        Nursery school to 4th grade,3:         5th grade or 6th grade,4:         7th grade or 8th grade,5:        9th grade,6:        10th grade,7:        11th grade,8:        12th grade, no diploma,9:        High school graduate,10:      Some college, but less than 1 year,11:      One or more years of college, no degree,12:     Associate degree,13:     Bachelor’s degree,14:     Master’s degree,15:         Professional degree, 16:         Doctorate degree.</td>

  </tr>

  <tr>

   <td width="153">income</td>

   <td colspan="2" width="463">Person’s total income.</td>

  </tr>

  <tr>

   <td width="153">latino</td>

   <td width="35">0:</td>

   <td width="429">Not Hispanic or Latino,</td>

  </tr>

  <tr>

   <td width="153"> </td>

   <td width="35">1:</td>

   <td width="429">Hispanic or Latino.</td>

  </tr>

  <tr>

   <td width="153">black</td>

   <td width="35">0:</td>

   <td width="429">Not Black or African American,</td>

  </tr>

  <tr>

   <td width="153"> </td>

   <td width="35">1:</td>

   <td width="429">Black or African American, alone or in combination with one or more other races.</td>

  </tr>

  <tr>

   <td width="153">asian</td>

   <td width="35">0:</td>

   <td width="429">Not Asian,</td>

  </tr>

  <tr>

   <td width="153"> </td>

   <td width="35">1:</td>

   <td width="429">Asian, alone or in combination with one or more other races.</td>

  </tr>

  <tr>

   <td width="153">married</td>

   <td width="35">0:</td>

   <td width="429">Presently married, not separated,</td>

  </tr>

  <tr>

   <td width="153"> </td>

   <td width="35">1:</td>

   <td width="429">Widowed, divorced, separated, never married.</td>

  </tr>

  <tr>

   <td width="153">divorced</td>

   <td width="35">0:</td>

   <td width="429">Married or not married but not divorced,</td>

  </tr>

  <tr>

   <td width="153"> </td>

   <td width="35">1:</td>

   <td width="429">Divorced and not remarried.</td>

  </tr>

  <tr>

   <td width="153">uscitizen</td>

   <td width="35">0:</td>

   <td width="429">Not a citizen of the United States,</td>

  </tr>

  <tr>

   <td width="153"> </td>

   <td width="35">1:</td>

   <td width="429">Citizen of United States.</td>

  </tr>

  <tr>

   <td width="153">children</td>

   <td width="35">0:</td>

   <td width="429">No own minor children living in residence,</td>

  </tr>

  <tr>

   <td width="153"> </td>

   <td width="35">1:</td>

   <td width="429">Lives with own minor children.</td>

  </tr>

  <tr>

   <td width="153">disability</td>

   <td width="35">0:</td>

   <td width="429">Without a disability,</td>

  </tr>

  <tr>

   <td width="153"> </td>

   <td width="35">1:</td>

   <td width="429">With a disability (sensory, physical, mental)</td>

  </tr>

  <tr>

   <td width="153">militaryservice</td>

   <td width="35">0:</td>

   <td width="429">No military service,</td>

  </tr>

  <tr>

   <td width="153"> </td>

   <td width="35">1:</td>

   <td width="429">Past or present active duty service, or training for reserves or National Guard.</td>

  </tr>

  <tr>

   <td width="153">employed</td>

   <td width="35">0:</td>

   <td width="429">Unemployed or not in labor force,</td>

  </tr>

  <tr>

   <td width="153"> </td>

   <td width="35">1:</td>

   <td width="429">Employed, including armed services.</td>

  </tr>

  <tr>

   <td rowspan="2" width="153">englishability fips</td>

   <td width="35">0: 1:</td>

   <td width="429">Spoken English ability is ”First Language”, ”Very Well” or ”Well”,4 Spoken English ability categorized as ”Not Well” or ”Not at all”.</td>

  </tr>

  <tr>

   <td colspan="2" width="463">Federal Information Processing Standards County Code.</td>

  </tr>

 </tbody>

</table>


