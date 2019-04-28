# FuzzyClips-Press-oTurboEmRela-oVelocidade
Trabalho de Inteligência Artificial - Pressão do Turbo em Relação a Velocidade terei tal explosão no motor

<p>
(deftemplate Velocidade <br/>
	0 200 Km_h<br/> 
	(<br/>
		(baixa (z 10 70)) <br/>
		(media (60 0)(80 1)(120 1)(150 0)) <br/>
		(alta (s 145 200)) <br/>
	) <br/>
)<br/>
</p>

<p>;(plot-fuzzy-value t "-*+" 0 200 (create-fuzzy-value Velocidade baixa) (create-fuzzy-value Velocidade media)(create-fuzzy-value Velocidade alta))</p>


<p>
(deftemplate PressaoTurbina <br/>
	0 100 Kilos <br/>
	(<br/>
		(fraca (z 1 40)) <br/>
		(media (pi 35 55)) <br/>
		(forte (s 75 100)) <br/>
	) <br/>
)<br/>
</p>


<p>;(plot-fuzzy-value t "-M+" 0 100 (create-fuzzy-value PressaoTurbina fraca) (create-fuzzy-value PressaoTurbina media) (create-fuzzy-value PressaoTurbina forte))
</p>

<h1>;Regras</h1>

<p>(defrule ExplosaoFraca <br/>
	(declare (salience 10)) <br/>
	(PressaoTurbina fraca) <br/>
	(Velocidade baixa) <br/>
	=> <br/>
	(assert (Explosao ExplosaoFraca)) <br/>
)<br/>
</p>

<p>(defrule ExplosaoConsideravel <br/>
	(declare (salience 10)) <br/>
	(or (and (PressaoTurbina media) (Velocidade baixa)) <br/>
		(and (PressaoTurbina fraca) (Velocidade media)) <br/>
	) <br/>
	=> <br/>
	(assert (Explosao ExplosaoConsideravel))<br/>
)<br/>
</p>



<p>(defrule ExplosaoMedia <br/>
	(declare (salience 10))<br/>
	(or (and (PressaoTurbina forte) (Velocidade baixa))<br/>
		(and (PressaoTurbina media) (Velocidade media))<br/>
		(and (PressaoTurbina fraca) (Velocidade forte))<br/>
	)<br/>
	=><br/>
	(assert (Explosao ExplosaoMedia))<br/>
)<br/>
</p>


<p>(defrule ExplosaoMediaAlta <br/>
	(declare (salience 10)) <br/>
		(or (and (PressaoTurbina forte) (Velocidade media)) <br/>
			(and (PressaoTurbina media) (Velocidade forte)) <br/>
		) <br/>
		=> <br/>
		(assert (Explosao ExplosaoMediaAlta)) <br/>
) <br/>
</p>


<p>(defrule ExplosaoAlta <br/>
	(declare (salience 10)) <br/>
	(PressaoTurbina forte) (Velocidade forte) <br/>
	=> <br/>
	(assert (Explosao ExplosaoAlta)) <br/>
)<br/>
</p>


<h1><p>(defglobal ?*g_resultado* = 0 )</p></h1> 

<p>(defrule defuzifica <br/>
	(declare (salience 0)) <br/>
	?v_tmp <- (Explosao ?) <br/>
	=> <br/>
	(bind ?*g_resultado* (moment-defuzzify ?v_tmp))	<br/>
	(plot-fuzzy-value t "*" nil nil ?v_tmp) <br/>
	(retract ?v_tmp) <br/>
	(printout t "Explosao da Turbina: ") <br/>
	(printout t ?*g_resultado* crlf) <br/>
	(printout t " >>> Programa Executado com Sucesso !! <<< " crlf) <br/>
)<br/>
</p>



;(deffacts fatos (PressaoTurbina fraca) (Velocidade baixa))
