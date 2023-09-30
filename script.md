# The Script

## (Cover page)

Hello, fellow scholars. I'm 张景致 and I'm going to talk about our work: Hiding Sensitive Information in Desensitized Voice Sequences. As you might notice, a total of four authors involve. Besides our mentor 闫红洋, there are 黄海, me and 陈迪轩. Okay, let's jump right in.

## Voice-based services

Voice is one of the most common and effective ways to communicate, the fact that I'm talking to you right now, instead of sending text messages, is the proof.

Generating utterance is a complex action. From thoughts in our brain, vibration of voice tract, to the shape of mouth, every single part of the process makes a difference. As a result, features in our speeches are abundant, thus we research to make use of it in computer system. 

Here you can see some of these voice-enabled products. There are voice assistants like Siri, and vocal input methods. The individual differences in our voices are even used as biometric security means.

## Speaker recognition

That's right, we have Speaker recognition, methods to identify someone from its vocal characteristics. We call such features voiceprint, it's distinct and measurable, perfect to label and describe people. With current advancement of AI technologies, it can be easily acquired.

As you can see the picture at the right-bottom corner is a spectrogram, one possible representation of voiceprint.

## Wait... I want just *what I asked*!

As you might notice by now, there is far too much information in our speeches. Yet generally our voice services focus on just a few aspects of them. There is a chance, a rather big chance, that our user just want to utilize some identity-neutral features, like textual content, instead of something that could lead to identification, the voiceprint. And of cource, users may, you know, for some reasons, talk about their passwords aloud.

And here I would love to mention that our work focus on voiceprint security. So the problem of, like speaking passwords, are out of our scope.

## Crucial privacy concerns

That's the problem. Unprocessed voice sequences contain everything, whether you want it or not. Have you pick up some random calls that sound horribly like someone familiar, and turned out to be a spam? Or ones where the caller instantly know who you are after you spoke? That's what may happen when your voiceprint is used for anything other than your locks.

## Mitigating identity privacy problem

How could we solve that? Well that sounds simple enough. If there isn't user's voiceprint, thieves have nothing practical to steal, right? And what I mean by removing voiceprint is really replacing it.

We have a few options. We can just shift the pitch and let us sound funny. But the results may experience big quality drop and, maybe, not secure enough. We could also just recognize the words, and let an AI speak them aloud. But in that way we would lose everything besides the words.

Really the best solution may lie in the world of neural networks. This field is well studied by many researchers and used by many products right now. And if you can gether better data and construct better, or just bigger models, you might just get better results. And you may probably aware that most of the hardware around us can accelerate models to a degree. The last but not least, they have goog chances to be of better security.

## Prior art - Voice conversion

And we have voice conversion method, StarGAN-VC, the StarGAN for voice conversion. To be clear, this is probably not the state of the art today, but it's popular among researches and we chose it for the experiment back when we were working on it.

It's many to many, non-parallel, which means that samples from different speakers need not to perfectly match each other in terms of words at a specific time. Above all, it produce resonably good results.

## Prior art - Identity protection

Also here we have a model for faces. It look similar to StarGAN-VC, but it's targeted at anonymization specifically, instead of generic conversion use cases. As we can see, this model preserves many features, including facial experession.

## Could anything go wrong?

Recall that our security measure brings up the need for a model, that replace the voiceprint to achive anonymity. When we want to evaluate such algorithm, the priority is to measure its performance, that's what we would need with it. If it's doing great, it might prove useful against dodgy service providers.

However, we may tend to overlook the possibility that the desensitization measure itself may fall victim to some kind of adversary. With some engineering, an attacker may be able to hide traces of original recording, like speaker's identity, in desensitized one. User would notice nothing if no specific consideration is put in analysis of such kind of threat. Even if user is aware, it might not recognize the method or technique used in such a adversary.

## Prior art - Subverting protection models

For PPRL-VGAN, the identity protection model I mentioned before, this possibility is already explored. As you can see here, with a few additional modules, using stenography techniques implemented as neural network layers, added to the workflow, a privacy perserving VGAN is turned into a privacy embedding one. Without noticable changes, the original identity of a desensitized image is readily available to attacker, to attacker only.

## Prior art - Stenography for speeches

For quite some time we talked about voices, and voiceprints. Now come the question, are there some kind of neural network stenography method for speeches? And here is Hide and Speak, an audio stenography mean that's specifically optimized for usage with speech data.

It utilizes multiple neural network modules and fourier transform functions to achieve the goal of writing one sequence of speech on top of another. It causes minimal changes to the carrier, and no audible distortion. As a bonus, it's robust to various common degradations like compression to MP-three, et cetera.

## Framework for our adversary

So here comes our framework of adversary. We first obtain original, or raw, speech from the user. And from user's point of view, the data goes into a desensitization mechanism, and returns as desensitized speech.

But the desensitized voice is not directly returned, instead it's embed with original data with a stenography model, so what really returns is a stenographic speech. When the user consumes online services with such data, the service provider would then able to extract the original voice from it and gain access to user's voiceprint.

## Implementation of our adversary

We now have a victim model and stenography algorithm readily available, where the actual anonymization is done by StarGAN-VC as a identity conversion process. Embedding and extracting process is then handled by Hide and Speak.

## Evaluation setup

We chose VCC twenty-sixteen, the same dataset used for training of StarGAN-VC, for our setup. And the actual implementation of it is StarGanVCDialectConversion, an PyTorch implementation available on GitHub. It's trained with official settings. For Hide and Speak, we used the official sources.

In order to provide accurate identity readings, we employ iFlyTek's online speaker recognition service, where each sample is given scores that resemble its probability of it being spoken by each speaker. It's worthwhile to mention that for this specific service, it consider a score greater than 0.6 as confident, where one sample can be consider confirmed to be spoken by this specific speaker.

## Evaluation experiment

We conducted experiment on training set, training set of our StarGAN-VC implemention. As we consider the generalization of victim model out of scope of our work, this shall not be our concern. Also, the model naturally perform better with training data, which may lead to clearer results for our evaluation.

Each sample in source produce 3 results in our workflow, thus we have a total of ninteen-forty-four results being generated for evaluation.

## Statistical metrics

We have 5 statistical mesurements for our results. All of them are performed on scores of original speaker's identity across each sample. Mean, best and worst scores are straightforward. Certainty counts scores higher than zero point six, which are confirmed by the service that they belong to the original identity. Class Ratio counts scores where the score for original speaker is higher than others. It means that typical classification models might classify it into the original speaker.

And here comes the results.

## Evaluation statistics

As you might notice from the first figure, the original recordings are all confidently classified into their original speakers, which proves that we didn't mess up with the dataset, and the service performs reasonably well.

For desensitized ones, the scores dropped drastically, which is intended. From the Best and Class Ratio values we can see there are still a portion of samples that appear to be not well desensitized. However, when we look at the almost zeroed Certainty, we can see that these samples can not be well classified into one specific speaker.

Looking at the stenographic samples, we can see that the changes are rather minimal. In some cases the desensitization performance even upgraded because of the distortion.

From the last figure we can see the measurements of extracted recordings are pretty close to what they should be in original samples, except for a slight drop in confidence. From our manual hearing test on a subset of them, we confirm that the degradation is not audiable.

That's all of our work, let's discuss what could happen next.

## Future work
Our fellow scholars may know already that StarGAN-VC may not really be state of the art. In fact there are better solution now, such as resemble dot ai's real time VC solution. It remain unclear whether these better solutions are more robust or not.

Also, the work we borrowed idea from, the work that subverted PPRL-VGAN, integrated their adversary into the victim as additional layers while in practice, ours are separate modules.

And of course, it's possible to find a way to defend or mitigate such adversary, or detect its presence.

Last but not least, one reviewer of our work revealed the opportunity of another use case for our adversary.

## What about devices without Internet?

There are lots of devices without active Internet connection. But the point is, many of them would have a chance to communicate with the outer world via a connected device. Let me give you an example. You have a door lock that reads your voice. And some day the vendor advised you to upgrade the firmware, and you need to use an app in your phone to do that for you, then a chance of communication is opened. In the process of upgrade, the device might upload some statistics and samples to the cloud, and all of your security measures found them nothing to do with you, would there be some kind of stenography going on? We won't get to know.

## Conclusion
We designed a workflow on hiding identity data in desensitized voice sequences, and conducted experiment on it. And we proved vulnerability of current solutions to such attack. It’s safe now to consider publicly-released voice changing solutions have potential for adversaries. And maybe, it's necessary to consider anti-stenography measures on desensitization models.

## Q&A - Ending
That's our presentation.
Any questions?
Thanks for listening!