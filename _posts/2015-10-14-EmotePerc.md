---
layout: post
title: Emotion Perception
---

<img align = "right" src="/PostImages/emotDims.png" width="50%" />
Humans misunderstand emotion from faces about 25% of the time, demonstrating differences across individuals.  I argue for a model of emotion perception where faces are represented in a perceptual space based on a few configural relations that are learned by experience with faces.  In fact, I determined what dimensions are used, and show them here. Below I define how I determined those relations and describe the theory in more detail.  In this post I argue that:

1. The perception of emotion from facial expressions of emotion is not fixed and universal, as suggested by Ekman and colleagues.  Rather, through typical experience we discover the key configural relations that allow perception of emotion from faces, with subtle differences according to individual experience that lead to errors in recognition.
2. The pattern of mistakes can be explained by a small set of relations between face features.  Moreover, our eye tracking data agreem with the findings from this model.  I.e., fixations often cluster near areas that were identified as important.
3. Certain emotions are more easily confused because of their similarity in perceptual space.
4. Humans develop their individual perceptual emotion space through experience with faces from infancy.  Although able to mimic and discriminate between various facial expressions of emotions from early on, experience with faces leads infants to learn what features are mportant for recognizing different emotions (See my SRCD 2013 for support).

### Support for this model
This view is consistent with several important and well established findings on face perception.

1. Neutral faces with altered eye-mouth distance are perceived as sadder/angrier although there is no muscle change (Neth and Martinez, 2009 ).  Thus, underlying muscle states as a route to recognizing emotion is unecessary (FACS), and simple first order relational face changes change the perceived emotional expression.
2. The low resolution/bubbles studies show that with a few simple relations we can perceive emotion.  The bubbles studies suggest distinct areas are used to recognize different emotions.  In my dissertation work, we found that this is unlikely since the pattern of gaze predicts emotion category far below participants' accuracy in recognizing emotion from faces.  Rather, our results and bubbles point to a common set of features that are used for recognizing emotion, with some features being more important for particular emotions.
3. The inverted face studies show that by manipulating the difficulty in measuring configural relations, face perception suffers.
4. Emotions from facial expressions can be perceived in a very short time and at very low resolution (cite cite), suggesting that simple measures may be sufficient for expression recognition.
5. Some cross cultural studies (old and recent) do show differences across culture (Jack et al.,2012; Shimoda, et al., 1978). Suggesting that experience helps in forming the perceptual emotion space.
6. Recent work by Du and colleagues ( Du et al., 2014) has shown that humans are able to perceive a wide variety of emotions from faces.  Our straightforward account of emotion perception is consistent with this view, and explains how we can learn any number of expressions by finding clusters in our internal emotion space.
7. Decades of cross-cultural studies have shown consistency of emotion recognition and categorical discrimination of facial expressions of emotion, but they suffer from a key problem.  The categorical response may be a result of the task (2AFC/3AFC), and may not reflect the actual emotion representation.  To be explicit, the emotion space may be a continuous blur, but if forced to chose between two options, participants will pick the closest one - resulting in a categorical-like pattern.


### Key Idea

The literature on face perception suggests that faces are interpreted holistically or configurally, and very quickly from even impoverished face images.  In addition, people make systematic mistakes when identifying the emotion being expressed in a face.  These observations along with the ones above suggest that there are some clearly identifiable measures of the face that are used to recognize emotion.  Those measures should not be influenced by skin texture, lighting, or other factors unrelated to expressing emotion.  Furthermore, there may be a common set of measures used across individuals that lead to the systematic pattern of errors exhibited by adults.  If those measures define the dimensions of a perceptual space of emotion recognition, then the different expressions should exist in different parts of that space.

We hypothesize that the measures used to parametrize the perceptual space of emotion are a common set of second order relations, or distances between face landmarks that are normalized by the overall face size.  Here, landmarks refer to 2D coordinates delineating the face shape.  Note that the distances between landmarks are first order relations, but normalizing by the face size makes them second order.

### Details and Implementation

Given our hypothesis, what are the relations that explain the pattern of emotion perception from faces?  To solve this problem, we formalize these ideas into a computational model that allows us to relate the configural measures of a face to the way it is percieved.  First, we define a representation for the values of faces along possible dimensions.  For the $$j^{th}$$ face in a set of $$N$$ expressive faces, $$\{F\}$$, we denote the value along the $$i^{th}$$ dimension by $$x_{ij}$$.  We assume that each $$i$$ corresponds to a pairwise distance between two face landmarks, but the formalism can be used to represent any face parametrization.  We denote the vector of all relations in the $$j^{th}$$ face by $$\mathbf{x}_j$$.

Next, we must define a representation for how a particular facial expression is perceived. This representation should be flexible enough to describe how any face is perceived, should allow comparison across faces, and should make different emotions orthogonal to each other such that we do not assume an ordering in the space. For convenience, we assume that expressive faces can be perceived as any of the six prototypical facial expressions of emotion (happy, sad, surprise, angry, disgust, fear), or neutral.  Then one possible representation is a 7-dimensional binary vector, with all zeros except for a one in the coordinate corresponding to the perceived emotion.  This is a good start, but insufficient in that it does not take into account disagreements about the perceived emotion in 25% of cases.  Therefore, we assume that after a set of raters have decided which emotion is being expressed in a face, each dimension should denote the proportion of participants who perceived the emotion corresponding to that dimension.  This is equivalent to taking the average of the binary representation for many raters.  Thus, the $$j^{th}$$ face in the set $$\{F\}$$ is associated with a seven dimensional vector, $$\mathbf{y}_j$$, describing how it is perceived on average.

This model then implies that for faces that are perceived similarly, i.e. the faces with $$\mathbf{y}_j$$ clustered together, the corresponding feature vectors $$\mathbf{x}_j$$ vectors should be cluster together.  Conversely, the pairs that are perceived differently will have $$\mathbf{y}_j$$ vectors farther apart, and $$\mathbf{x}_j$$ vectors farther apart. The problem of finding which relations lead to the pattern of emotion perception then amounts to finding the indices for a set  of relations, $$\{I\}$$ that optimize the following objective:

$$\arg \min_I\frac{2}{N^2-N}\sum_{m=2,n=1}^{m=N,n=m-1}(\frac{1}{d}\|\mathbf{x}^I_m-\mathbf{x}^I_n \|_2^2 - \|\mathbf{y}_m-\mathbf{y}_n \|_2^2  )^2,$$  
$$\arg \min_I\frac{1}{N^2}\sum_{m=1,n=1}^{m=N,n=N}(\frac{1}{d}\|\mathbf{x}^I_m-\mathbf{x}^I_n \|_2^2 - \|\mathbf{y}_m-\mathbf{y}_n \|_2^2  )^2,$$

where $$\mathbf{x}^I_j$$ corresponds to the vector of features in $$\{I\}$$ for the $$j^{th}$$ face.  Normalizing by $$d$$ ensures that smaller numbers of features are not preferred.

The objective above is essentially that of generalized multidimensional scaling  ( Bronstein, et al., 2006), which determines a projection of data to a lower dimensional manifold that preserve distances in some similarity space.  In our case, instead of determining an optimal embedding, we wish to determine which relations form the set $I$ that gives the lowest error.  To solve, we take a greedy incremental feature selection approach.  We start with an empty set $I$, and on every iteration of the optimization algorithm, we find the relation in an exhaustive set of all vertical and horizontal relations that minimizes the error when added to the set.  That relation is then added to $$I$$ for the subsequent iteration.  We assume all face shapes are first normalized to horizontal based on eye center, and a unit inter-eye distance.  We also assume that each face shape is associated with with a rating vector $$\mathbf{y}$$ that describes how it is perceived by participants.

### Emotion rating task
I used a behavior and eye tracking data set collected as part of my dissertation work. Studies were approved by IRB, and participants gave written consent prior to participating. There,  we were investigated patterns in gaze when classifying expressions of emotion.  In that work, participants did a seven alternative force choice (AFC) task where they first saw face images depicting all 7 expressions with labels, and had one practice round where they saw and identified all of the emotions from the practice round stimuli.  Participants then viewed a series of faces of different individuals expressing the 6 prototypical emotion expressions + neutral (happy, sad, surprise, fear, disgust, anger).  Participants saw a list of the 7 possible choices after each image, and identified the viewed expression by looking at the option that they thought was expressed and pressing a button on a controller.  This task gave us the different faces $$\mathbf{x}$$ with their corresponding perceptual ratings, $$\mathbf{y}$$. Thus, we could solve the objective function proposed above, and identify the configural relations that people use when they recognize emotion from faces.  We show the 5 first relations above.

#### Bibliography
Bronstein AM, Bronstein MM, Kimmel R (2006). "Generalized multidimensional scaling: a framework for isometry-invariant partial surface matching". _PNAS_ 103(5): 1168–72.

D. Neth and A.M. Martinez (2009). Emotion Perception in Emotionless Face Images Suggests a Norm-based Representation. _Journal of Vision_. 9(1), pp 1-11

Rachael E. Jack, Oliver G. B. Garrod, Hui Yu, Roberto Caldara, and Philippe G. Schyns (2012). Facial expressions of emotion are not culturally universal. _PNAS_; published ahead of print April 16, 2012

Shichuan Du, Yong Tao, and Aleix M. Martinez (2014) Compound facial expressions of emotion, _PNAS_

Shimoda,K. et al (1978). The intercultural recognition of emotional expressions by three national racial groups: English, Italian and Japanese.  _Europ Journal of Social Psych_



