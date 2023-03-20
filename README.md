# max-shadertoy
Versions Cycling ’74 Max (version 8.5 minimum) de nuanceurs Shadertoy.

## Introduction

Shadertoy.com est un site Web populaire pour partager des démos de nuanceurs. Beaucoup d'entre eux sont animés et s'appuient sur les nouvelles fonctionnalités GLSL. 

Ce dépôt redstribue le fichier JXS partagé avec la version 8.5 de Max qui permet d'inclure les fonctionnalités de base des effets Shadertoy à tampon unique et regroupe aussi les nuanceurs convertis par les étudiants du programme de Techniques d'intégration multimédia.

## Code de base du nuanceur

Coller le code du nuanceur Shadertoy entre les commentaires `// <<<<<<<<< START SHADERTOY PASTE ` et `// <<<<<<<<< END SHADERTOY PASTE` du fichier JXS suivant :

```
<jittershader name="stripes">
    <param name="modelViewProjectionMatrix" type="mat4" state="MODELVIEW_PROJECTION_MATRIX" />
    <param name="textureMatrix0" type="mat4" state="TEXTURE0_MATRIX" />
    <param name="position" type="vec3" state="POSITION" />
    <param name="texcoord" type="vec2" state="TEXCOORD" />
    <param name="iResolution" type="vec2" state="TEXDIM0" />
    <param name="iMouse" type="vec4" default="0 0 0 0" />
    <param name="iTime" type="float" default="0" />
	<param name="dummytex" type="int" default="0" />
	<param name="iChannel0" type="int" default="1" />
	<param name="iChannel1" type="int" default="2" />
	<param name="iChannel2" type="int" default="3" />
    <param name="iChannel3" type="int" default="4" />
    <language name="glsl" version="1.5">
        <bind param="modelViewProjectionMatrix" program="vp" />
        <bind param="textureMatrix0" program="vp" />
        <bind param="position" program="vp" />
        <bind param="texcoord" program="vp" />
        <bind param="iTime" program="fp" />
        <bind param="iMouse" program="fp" />
        <bind param="iResolution" program="fp" />
		<bind param="dummytex" program="fp" />
		<bind param="iChannel0" program="fp" />
		<bind param="iChannel1" program="fp" />
		<bind param="iChannel2" program="fp" />
        <bind param="iChannel3" program="fp" />
		<program name="vp" type="vertex"  >
		<![CDATA[
			#version 330 core
			
			in vec3 position;
			in vec2 texcoord;
			out jit_PerVertex {
				vec2 texcoord;
			} jit_out;
			uniform mat4 modelViewProjectionMatrix;
			uniform mat4 textureMatrix0;
			
			void main(void) {
				gl_Position = modelViewProjectionMatrix*vec4(position, 1.);
				jit_out.texcoord = vec2(textureMatrix0*vec4(texcoord, 0., 1.));
			}
		]]>
		</program>      
        <program name="fp" type="fragment"  >
        <![CDATA[
            #version 330 core
            
            in jit_PerVertex {
                vec2 texcoord;
            } jit_in;
            out vec4 outColor;

            uniform float iTime;
            uniform vec2 iResolution;
            uniform vec4 iMouse;
			uniform sampler2DRect dummytex;
			uniform sampler2D iChannel0;
			uniform sampler2D iChannel1;
			uniform sampler2D iChannel2;
            uniform sampler2D iChannel3;

// <<<<<<<<< START SHADERTOY PASTE


// <<<<<<<<< END SHADERTOY PASTE

			void main(void) {                
				mainImage(outColor, jit_in.texcoord.st);
			}
        ]]>
        </program>
    </language>
</jittershader>

```
