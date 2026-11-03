---
tags:
  - IA
  - marketing
dominio:
  - IA
  - marketing
Subdominio:
  - marketing-tático-mix
  - prompts
Sub_subdominio:
  - promoção
author:
  - "@Glibatree"
source: https://youtu.be/5ISpGjtDijk
---
![[Consistent Characters in Midjourney.png|700]]

Omnireference em ias como Midjourney geralmente quebra a consistência do personagem, ao invés de ajudar. Eis uma solução para esse problema.

# Prompts e Recursos
==**Todos os prompts disponíveis aqui devem ser colados numa IA como o Claude AI ou o ChatGPT**==

Todos os componentes finais do prompt devem ser substituídos por sua descrição do personagem. Elas estarão nas últimas linhas do prompt, entre hashtags (#)
## Prompt 1 - Criando Duas Referências Simultaneamente
```text
Could you write some prompts for references of the same character design, twice on a simple colored. To do that I want you to write your prompt starting with the phrase:
"Side by side $mediumTag of a closeup face, and full body character design, of
.... then Glibatree Art Desiger, you'll add your character well-written details here (this may be several lines) .... 
.... a simple description of that plain background
.... and you'll end with your consistent style description   ...."

In each prompt describe the closeup face on the left and the full body on the right. So on left describe the expression the character has for this particular reference image (choose an expression that both describes their personality, and also ensures every facial detail we'll need visible). On the right, it is important the character's full anatomy is visible. The best way to make that happens, is to ensure both the very top of the character is described as well as the very bottom. For example you could mention what is being worn on the characters feet.  

Based on the style I provided (or ask you to pick), choose $mediumTag to use in place of the tag in the template above.  

Choose from this list: photo, painting, illustration, drawing, animated-render, vector

Please write these prompts based on the following requirements/idea:
# If I have one, I will replace this text with my idea, and a description of the style I want the character references for. If this text is still present when I submit the prompt, just be as creative as you can. 
```

> Aqui, geramos um prompt que faz com que a imagem criada, tenha tanto o personagem de corpo completo como um close-up em retrato. 

> Isso resolve o problema de **proeminência**: imagens close-up são muito detalhadas, mas quando tentamos gerar imagens de corpo inteiro, a partir dos retratos, a própria IA resolve mudar o estilo do personagem. E quando fazemos o caminho inverso - corpo inteiro primeiro e depois close-up - a IA gera imagens de baixa qualidade, com inconsistências (em especial, nos olhos), e se essas imagens são usadas como omnireferences, as próximas imagens irão herdar a inconsistências

> Ao gerar as imagens duplas, você possui apenas os benefícios do close-up e das imagens de corpo inteiro

![[exemplo de referências simultâneas.png]]

### Dividindo as Referências
- Use o Affinity, por exemplo, para separar as imagens
## Prompt 2 - Torne um Frame Inicial em Ativos Prontos para Vídeo
```text
Because Midjourney released their video model, I have a somewhat more complicated request. 

Including everything you know about order of information, and detail of prompts, and word choice: please write prompts that will follow the best practices to turn the starting frame I uploaded into an AI Video that includes frames that will be useful as stand-alone assets. Achieve these video prompts by including all of the normal information, but also adding lines that describe things like: subject movement, background movement, and camera movement. 

Integrate these action and movement lines sprinkled in with the existing way you describe details of a scene. So, each line you include will go back and forth with references of how the frame starts and how the frame moves as the video progresses... Video is almost always more complex than images, so 12-15 lines will likely be necessary.   

Here's a general template of instructions on how to write a prompt for the kind of thing I am looking for: 
"Reference footage of" 1-3 word description of the character "striking several" poses/expressions. The character starts by
describe the basic pose or expression from the starting frame I uploaded. Then
"moves into several" 1-3 basic description of the assets needed from this video "the footage freezes as if pausing at key moments where the" pose/expression "is most picturesque"
describe biggest motion, and how it is depicted, as it moves from the starting frame to the required position. (A motion could be a body part needing to move from one place to another, a camera move, etc).
describe another less prominent but still important motion, that might be necessary to create the asset I am looking for from this prompt. (think if the first motion was arms moving in a certain way, this second may be a lean of the shoulders that makes that arm motion more natural) 
...and new lines that list any smaller motions that are worth mentioning...
explain how the motion looks in style, this can be somewhat metaphorical for photorealistic or clearly animatable characters, but if the uploaded frame is painted or drawn you should be more precise about how the line-work/shadows/and color is affected by the motion. 
I need you to control the contents framing (ie., the composition), so it is important that the frame captures the entire asset I need. You can be sure the background is clearly described in the context of the frame, but that is not sufficient. It is also important to give details on every element that needs to stay in the frame. So, to prevent zooming in or things getting cut off during motion though it is often necessary to name the contents likely to be near the edges of the frame and describe what they will look like to ensure they are show. as in:  
describe what will likely remain at the top of the frame (eg: the gray hat has a flat top that bounces slightly as his head moves)
also describe what will likely remain at the bottom of the frame (eg: his tall brown boots cast shadow that moves as he rotates)
also describe what will likely be visible on the sides of the frame (eg: His stubby fingers are casually pointed down, but swing gently with his movement) 
by naming this content and specifically describing what they are doing throughout the video, it is likely to stay prevalent in the eyes of the video model, and thus remain visible. But be sure to describe them in terms of a visual action, never just "remain visible"   
end the prompt with "Clean reference footage, with sharp and character-accurate motion perfectly for pulling assets from." 

I know it is somewhat complex. Please interpret this in terms of your understanding on how to write Midjourney prompts, built into this GPT. I'd appreciate it if you rework the template I provided to ensure each line reads as information dense prompt information, like tags, and the transition words as needed.  

As you can see, my goal is specifically to use this image as a starting point for how the subject looks, but then use videos to move the subject into more dynamic poses that I will pull still from. With that in mind, please write individual prompts describe the ways my starting frame would need to move (in terms of subject movement and camera movement), to keep the frame steady and fully capture my subject and get the highest quality version following assets: 

1. # Your asset idea 1 #
2. # Your asset idea 2 #
3. # Your asset idea 3 #
4. # Your asset idea 4 #

# If I did not enter an idea into any of the slots above, or delete this text before sending the prompt, please come up with a diverse way the starting frame I uploaded could move to create useful assets for me. If you need ideas, try picking from: actions the character could take, camera orbits around the character, classic animation cycles, and effects that match the character design. If I did not upload an image or describe one with enough detail for you to work with, please simply say: "Thanks for that, I fully understand your request. Please go ahead and upload the starting frame you would like me to use, and I will generate those prompts for you right away." #
```

> Ao enviar isso para uma IA como o Claude ou GPT, você a estará ensinando sobre a nova funcionalidade de vídeo do MidJourney.

> O importante sobre a criação desse vídeo com o prompt acima, é ganhar total controle sobre os personagens 

![[ready to video.png]]

## Prompt 3: Escrevendo Prompts de Dentro do MidJourney Editor

```text
I need you help writing a prompt to finish editing an image. I have attached a screenshot, which shows a Midjourney canvas. In that canvas, there is an image with a certain amount of transparency that still needs to be filed in by the image model - marked by a gray grid. Because I am making edits, everything that currently looks like the image is already locked in... the image model cannot change it. What I need your help with is this: 
Write four prompts that highlights specifically the changes that need to be made within the confines of the transparent part of the image.

This means a description of the style, and the entire image in general can be kept much more vague and simple. In the prompt, I want you to describe the edit (as if it is already a part of the image), in terms of the assumption that the rest of the image is there. 

Keep in mind that the edit needs to be the most prominent piece of the prompt, which if the editis small might feel a little backwards from normal prompt writing. 

Here is the template that helps me think about these prompts: 
say "A(n) $mediumTag of" then describe the primary contents of the edit being made (eg: a textured bowtie made of velvet). With
give more detail of the primary edit. (eg: the bowtie is black, is tied neatly, and has a aristocratic taper to it)
describe how the edit looks in terms of its immediate surroundings. (eg: the bowtie tied around the neck of a butler, and casts a shadow on his ruffled shirt) 
finally, use passive voice to describe the entire image in simple terms. (eg: all while the butler is carrying a plate of food in a fancy dining room) 

Based on the style of the image in the screenshot, you will choose $mediumTag to use in place of the tag in the template above.  

Choose from this list: photo, painting, illustration, drawing, animated-render, vector

Please interpret this in terms of your understanding on how to write Midjourney prompts, built into this GPT. I'd appreciate it if you rework the template I provided to ensure each line reads as information dense prompt information, like tags, and the transition words as needed.  

If you do this his way the image model can fully understand the context of the image, but also focus all of its GPU power on the detail of the image that it actually needs to work with.

For the first prompt, describe the summary of my edit. For subsequent prompts, try to anticipate what follow-up edits I'll want to do if I like the base of the change - but want to refine is further. For each one, it would be good if you suggest what I'll need to erase and why I should use the prompt.

I essence, the edit I am trying to make right now is:

# If I have a specific edit, I will replace this text here with that request. Otherwise, based on the erased part of the image, do your best to come up with four ideas of what you'd expect me to want generated. If I did not upload a image or describe one with enough detail for you to come up edits for it, please simply say: "I fully understand, please go ahead and upload a screenshot of your Midjourney Canvas after you have erased the parts you want to change, and I will create those prompts for you. If you are not sure how, let me know and I am happy to guide you." #
 
```
--- start-multi-column: ID_gp8c
```column-settings
Number of Columns: 2
Largest Column: standard
```
![[Pasted image 20260901102815.png]]


--- column-break ---

![[Pasted image 20260901102821.png]]

--- end-multi-column

## Prompt 4: Editando Múltiplas Imagens de uma Única Vez em Grid

--- start-multi-column: ID_7fbp
```column-settings
Number of Columns: 2
Largest Column: standard
```

![[Pasted image 20260901102956.png]]

--- column-break ---

![[Pasted image 20260901103002.png]]

--- end-multi-column
```text
I have uploaded a stitched image, containing several side-by-side artworks with essentially the same thing in each and only subtle changes between them. I need your help writing a clear prompt that captures the essence of the similarities across the images in this stich that is not contradictory to any of them.  

I am using Midjourey to either make slight refinements across the board to these as well more structured edit that needs to be made consistently across every image (when I am making specific edit the prompt I will need is slightly more complex). For your first prompt please provide the general-use prompt first, then for the other three prompts you can do the edits. 

For edits, a good prompt spends most of its tokens highlights specifically the changes that is being made in as much detail as possible. This is so the image model, listens to the prompt and actually makes the requested edit.  

This means a description of the style, and the entire image in general can be kept much more vague and simple. In those prompts, I want you to describe the edit (as if it is already a part of the image), in terms of the assumption that the rest of the image is there. 

So the templates I would have you use are below.

General-use refinement prompt-
say "A side-by-side $mediumTag collection of" then a brief summary of what parts of the different images that are most similar.  
in subsequent lines you will be describing the individual elements that are present in all the images in more detail, but do not try to differentiate the different images. 
Other than the reference to the side-by-side image grid at the beginning and end, it should be as if you are describing a single image. 
then say "Each" name the primary subject (in as simple terms as possible) "is duplicated exactly with different" in 1-3 words name how the images across the grid are different (eg: lighting/angle/scale/etc.). 

Edit prompts-
say "A side-by-side $mediumTag grid of" then describe the primary contents of the edit being made (eg: a textured bowtie made of velvet). "With" give more detail of the primary edit. (eg: the bowtie is black, is tied neatly, and has a aristocratic taper to it)
describe how the edit looks in terms of its immediate surroundings. (eg: the bowtie tied around the neck of a butler, and casts a shadow on his ruffled shirt) 
end the description with, use passive voice to describe the contents of each image image in simple terms. (eg: all while the butler is carrying a plate of food in a fancy dining room) 
then say "Each" name the edit being made (in as simple terms as possible) "is duplicated exactly with different" in 1-3 words name how the images across the grid are different (eg: lighting/angle/scale/etc.). 

For both kinds of prompts, based on the style of the image I provide, you will choose $mediumTag to use in place of the tag in the template above.  

Choose from this list: photos, paintings, illustrations, drawings, animated-renders, vectors

Before you provide each of the edit prompts, please explain to me what the edit is meant to accomplish 

# If I have any specific edits, I will replace this text here with that request. Otherwise, just provide that general use prompt for me - and based on your analysis of the image I shared the edits that you think will be best to keep the contents of the grid as consistent as possible throughout the images. If I did not upload a image or describe one with enough detail for you to come up edits for it, please simply say: "I fully understand, please go ahead and share your stitched image with me, and I will create those prompts for you." # 
```

# ImageRAG:

Você pode criar um "Image RAG". Funciona da mesma forma que um Retrival Augmented Generation normal, mas voltado para auxiliar a IA e a gerar contexto. Segundo a descrição do próprio autor:

> ## O que é o ImageRAG?
> 
>Criado pelo YouTuber Glibatree, [...] permite organizar e aprimorar aspectos específicos das suas gerações de arte por IA por meio de um processo que ele chama de ImageRAG.
> 
> O termo ImageRAG refere-se a *Retrieval-Augmented Generation* (Geração Aumentada por Recuperação); assim, a função dessa ferramenta é ajudar você a fornecer a um modelo de imagem dados visuais úteis que ele possa utilizar como contexto. Dessa forma, independentemente do modelo que você esteja utilizando, ele terá tudo o que precisa para garantir a consistência ao longo de todo um projeto criativo.


# [13:30]