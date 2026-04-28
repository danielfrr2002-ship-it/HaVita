// ============================================================
// HAVITA — Sistema de Habitabilidad Temporal
// Identidad Visual Completa + Encuesta + Resultados
// Flutter — Single File Implementation
// ============================================================
//
// Estructura de archivos sugerida en proyecto real:
//   lib/
//   ├── main.dart
//   ├── theme/
//   │   └── havita_theme.dart
//   ├── widgets/
//   │   ├── hv_button.dart
//   │   ├── hv_chip.dart
//   │   ├── hv_index_bar.dart
//   │   └── hv_progress_header.dart
//   ├── models/
//   │   └── evaluation_result.dart
//   ├── services/
//   │   └── evaluation_service.dart
//   └── screens/
//       ├── intro_screen.dart
//       ├── survey_screen.dart
//       └── result_screen.dart
// ============================================================

import 'package:flutter/material.dart';
import 'package:flutter/services.dart';

// ──────────────────────────────────────────────────────────
// ENTRY POINT
// ──────────────────────────────────────────────────────────
void main() {
  WidgetsFlutterBinding.ensureInitialized();
  SystemChrome.setSystemUIOverlayStyle(
    const SystemUiOverlayStyle(
      statusBarColor: Colors.transparent,
      statusBarIconBrightness: Brightness.light,
      systemNavigationBarColor: HVColors.black,
      systemNavigationBarIconBrightness: Brightness.light,
    ),
  );
  runApp(const HaVitaApp());
}

// ──────────────────────────────────────────────────────────
// DESIGN TOKENS
// ──────────────────────────────────────────────────────────
class HVColors {
  // Core palette
  static const Color black       = Color(0xFF0A0A0A);
  static const Color blackSurf   = Color(0xFF111111);
  static const Color blackCard   = Color(0xFF181818);
  static const Color blackBorder = Color(0xFF252525);
  static const Color blackMid    = Color(0xFF2E2E2E);

  static const Color white       = Color(0xFFF2F2F0);
  static const Color whiteDim    = Color(0xFF999996);
  static const Color whiteFaint  = Color(0xFF444440);

  // Accent: signal red
  static const Color red         = Color(0xFFE03030);
  static const Color redDim      = Color(0x33E03030);
  static const Color redGlow     = Color(0x80E03030);

  // Semantic results
  static const Color accepted    = Color(0xFF22C55E);  // green
  static const Color acceptedDim = Color(0x2222C55E);
  static const Color conditional = Color(0xFFFBBF24);  // amber
  static const Color conditDim   = Color(0x22FBBF24);
  static const Color notViable   = Color(0xFFE03030);  // red
  static const Color notViableDim= Color(0x22E03030);
  static const Color priority    = Color(0xFF3B82F6);  // blue
  static const Color priorityDim = Color(0x223B82F6);
}

class HVType {
  static const String display = 'Barlow Condensed'; // Display font
  static const String body    = 'DM Sans';
  static const String mono    = 'DM Mono';
}

// ──────────────────────────────────────────────────────────
// THEME
// ──────────────────────────────────────────────────────────
ThemeData buildHaVitaTheme() {
  return ThemeData(
    brightness: Brightness.dark,
    scaffoldBackgroundColor: HVColors.black,
    colorScheme: const ColorScheme.dark(
      primary: HVColors.red,
      surface: HVColors.blackSurf,
      onSurface: HVColors.white,
      error: HVColors.red,
    ),
    fontFamily: HVType.body,
    useMaterial3: true,
    appBarTheme: const AppBarTheme(
      backgroundColor: Colors.transparent,
      elevation: 0,
      surfaceTintColor: Colors.transparent,
      iconTheme: IconThemeData(color: HVColors.white),
      titleTextStyle: TextStyle(
        fontFamily: HVType.display,
        fontSize: 20,
        fontWeight: FontWeight.w700,
        letterSpacing: 1.5,
        color: HVColors.white,
      ),
    ),
    textTheme: const TextTheme(
      displayLarge: TextStyle(
        fontFamily: HVType.display,
        fontSize: 52,
        fontWeight: FontWeight.w900,
        height: 0.95,
        letterSpacing: -0.5,
        color: HVColors.white,
      ),
      displayMedium: TextStyle(
        fontFamily: HVType.display,
        fontSize: 38,
        fontWeight: FontWeight.w800,
        height: 1.0,
        letterSpacing: 0.5,
        color: HVColors.white,
      ),
      headlineLarge: TextStyle(
        fontFamily: HVType.display,
        fontSize: 28,
        fontWeight: FontWeight.w700,
        letterSpacing: 1.0,
        color: HVColors.white,
      ),
      headlineMedium: TextStyle(
        fontFamily: HVType.display,
        fontSize: 20,
        fontWeight: FontWeight.w700,
        letterSpacing: 1.5,
        color: HVColors.white,
      ),
      titleLarge: TextStyle(
        fontFamily: HVType.body,
        fontSize: 17,
        fontWeight: FontWeight.w500,
        height: 1.45,
        color: HVColors.white,
      ),
      bodyLarge: TextStyle(
        fontFamily: HVType.body,
        fontSize: 15,
        fontWeight: FontWeight.w400,
        height: 1.6,
        color: HVColors.whiteDim,
      ),
      bodyMedium: TextStyle(
        fontFamily: HVType.body,
        fontSize: 13,
        fontWeight: FontWeight.w400,
        height: 1.55,
        color: HVColors.whiteDim,
      ),
      labelLarge: TextStyle(
        fontFamily: HVType.mono,
        fontSize: 11,
        fontWeight: FontWeight.w500,
        letterSpacing: 1.8,
        color: HVColors.whiteDim,
      ),
      labelSmall: TextStyle(
        fontFamily: HVType.mono,
        fontSize: 10,
        letterSpacing: 2.0,
        color: HVColors.whiteFaint,
      ),
    ),
  );
}

// ──────────────────────────────────────────────────────────
// APP
// ──────────────────────────────────────────────────────────
class HaVitaApp extends StatelessWidget {
  const HaVitaApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'HaVita',
      debugShowCheckedModeBanner: false,
      theme: buildHaVitaTheme(),
      home: const IntroScreen(),
    );
  }
}

// ──────────────────────────────────────────────────────────
// REUSABLE DESIGN COMPONENTS
// ──────────────────────────────────────────────────────────

// HV Surface Card — the modular building block
class HVCard extends StatelessWidget {
  final Widget child;
  final EdgeInsets? padding;
  final Color? borderColor;
  final Color? background;
  final double? borderWidth;

  const HVCard({
    super.key,
    required this.child,
    this.padding,
    this.borderColor,
    this.background,
    this.borderWidth,
  });

  @override
  Widget build(BuildContext context) {
    return Container(
      padding: padding ?? const EdgeInsets.all(20),
      decoration: BoxDecoration(
        color: background ?? HVColors.blackCard,
        border: Border.all(
          color: borderColor ?? HVColors.blackBorder,
          width: borderWidth ?? 1.0,
        ),
      ),
      child: child,
    );
  }
}

// HV Label — mono uppercase tag
class HVLabel extends StatelessWidget {
  final String text;
  final Color? color;

  const HVLabel(this.text, {super.key, this.color});

  @override
  Widget build(BuildContext context) {
    return Text(
      text.toUpperCase(),
      style: Theme.of(context).textTheme.labelSmall?.copyWith(
        color: color ?? HVColors.whiteFaint,
        letterSpacing: 2.5,
      ),
    );
  }
}

// HV Divider — structural separator
class HVDivider extends StatelessWidget {
  final Color? color;
  const HVDivider({super.key, this.color});

  @override
  Widget build(BuildContext context) {
    return Container(
      height: 1,
      color: color ?? HVColors.blackBorder,
    );
  }
}

// HV Primary Button
class HVButton extends StatefulWidget {
  final String label;
  final VoidCallback? onPressed;
  final bool outlined;
  final IconData? icon;
  final bool fullWidth;
  final Color? accentColor;

  const HVButton({
    super.key,
    required this.label,
    this.onPressed,
    this.outlined = false,
    this.icon,
    this.fullWidth = true,
    this.accentColor,
  });

  @override
  State<HVButton> createState() => _HVButtonState();
}

class _HVButtonState extends State<HVButton>
    with SingleTickerProviderStateMixin {
  late AnimationController _pressCtrl;
  late Animation<double> _scale;

  @override
  void initState() {
    super.initState();
    _pressCtrl = AnimationController(
      vsync: this,
      duration: const Duration(milliseconds: 100),
      lowerBound: 0.96,
      upperBound: 1.0,
      value: 1.0,
    );
    _scale = _pressCtrl;
  }

  @override
  void dispose() {
    _pressCtrl.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    final accent = widget.accentColor ?? HVColors.red;
    final enabled = widget.onPressed != null;

    return GestureDetector(
      onTapDown: enabled ? (_) => _pressCtrl.reverse() : null,
      onTapUp: enabled
          ? (_) {
              _pressCtrl.forward();
              widget.onPressed?.call();
            }
          : null,
      onTapCancel: () => _pressCtrl.forward(),
      child: ScaleTransition(
        scale: _scale,
        child: Container(
          width: widget.fullWidth ? double.infinity : null,
          padding: const EdgeInsets.symmetric(vertical: 16, horizontal: 24),
          decoration: BoxDecoration(
            color: widget.outlined
                ? Colors.transparent
                : enabled
                    ? accent
                    : HVColors.blackMid,
            border: Border.all(
              color: enabled ? accent : HVColors.blackBorder,
              width: 1.0,
            ),
          ),
          child: Row(
            mainAxisAlignment: MainAxisAlignment.center,
            mainAxisSize:
                widget.fullWidth ? MainAxisSize.max : MainAxisSize.min,
            children: [
              if (widget.icon != null) ...[
                Icon(widget.icon,
                    color: widget.outlined ? accent : HVColors.white, size: 18),
                const SizedBox(width: 10),
              ],
              Text(
                widget.label.toUpperCase(),
                style: TextStyle(
                  fontFamily: HVType.display,
                  fontSize: 14,
                  fontWeight: FontWeight.w700,
                  letterSpacing: 2.0,
                  color: widget.outlined
                      ? (enabled ? accent : HVColors.whiteFaint)
                      : (enabled ? HVColors.white : HVColors.whiteFaint),
                ),
              ),
            ],
          ),
        ),
      ),
    );
  }
}

// HV Chip — answer selector
class HVChip extends StatefulWidget {
  final String label;
  final bool selected;
  final VoidCallback onTap;
  final Color? selectedColor;

  const HVChip({
    super.key,
    required this.label,
    required this.selected,
    required this.onTap,
    this.selectedColor,
  });

  @override
  State<HVChip> createState() => _HVChipState();
}

class _HVChipState extends State<HVChip>
    with SingleTickerProviderStateMixin {
  late AnimationController _ctrl;
  late Animation<double> _scale;

  @override
  void initState() {
    super.initState();
    _ctrl = AnimationController(
      vsync: this,
      duration: const Duration(milliseconds: 80),
      lowerBound: 0.94,
      upperBound: 1.0,
      value: 1.0,
    );
    _scale = _ctrl;
  }

  @override
  void dispose() {
    _ctrl.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    final accent = widget.selectedColor ?? HVColors.red;

    return GestureDetector(
      onTapDown: (_) => _ctrl.reverse(),
      onTapUp: (_) {
        _ctrl.forward();
        HapticFeedback.selectionClick();
        widget.onTap();
      },
      onTapCancel: () => _ctrl.forward(),
      child: ScaleTransition(
        scale: _scale,
        child: AnimatedContainer(
          duration: const Duration(milliseconds: 180),
          curve: Curves.easeOut,
          padding: const EdgeInsets.symmetric(horizontal: 20, vertical: 14),
          decoration: BoxDecoration(
            color: widget.selected ? accent : HVColors.blackCard,
            border: Border.all(
              color: widget.selected ? accent : HVColors.blackBorder,
              width: widget.selected ? 1.0 : 1.0,
            ),
          ),
          child: AnimatedDefaultTextStyle(
            duration: const Duration(milliseconds: 180),
            style: TextStyle(
              fontFamily: HVType.body,
              fontSize: 14,
              fontWeight:
                  widget.selected ? FontWeight.w500 : FontWeight.w400,
              color: widget.selected ? HVColors.white : HVColors.whiteDim,
            ),
            child: Text(widget.label),
          ),
        ),
      ),
    );
  }
}

// HV Progress Header
class HVProgressHeader extends StatelessWidget {
  final int current;
  final int total;
  final String blockLabel;
  final Animation<double> progressAnim;

  const HVProgressHeader({
    super.key,
    required this.current,
    required this.total,
    required this.blockLabel,
    required this.progressAnim,
  });

  @override
  Widget build(BuildContext context) {
    return Column(
      crossAxisAlignment: CrossAxisAlignment.start,
      children: [
        Row(
          mainAxisAlignment: MainAxisAlignment.spaceBetween,
          children: [
            HVLabel(blockLabel),
            HVLabel('$current / $total'),
          ],
        ),
        const SizedBox(height: 8),
        Container(
          height: 2,
          width: double.infinity,
          color: HVColors.blackBorder,
          child: AnimatedBuilder(
            animation: progressAnim,
            builder: (_, __) => FractionallySizedBox(
              widthFactor: progressAnim.value,
              alignment: Alignment.centerLeft,
              child: Container(color: HVColors.red),
            ),
          ),
        ),
      ],
    );
  }
}

// HV Index Bar — animated result indicator
class HVIndexBar extends StatefulWidget {
  final String label;
  final double value; // 0–10
  final Color color;
  final Duration delay;

  const HVIndexBar({
    super.key,
    required this.label,
    required this.value,
    required this.color,
    this.delay = Duration.zero,
  });

  @override
  State<HVIndexBar> createState() => _HVIndexBarState();
}

class _HVIndexBarState extends State<HVIndexBar>
    with SingleTickerProviderStateMixin {
  late AnimationController _ctrl;
  late Animation<double> _anim;

  @override
  void initState() {
    super.initState();
    _ctrl = AnimationController(
      vsync: this,
      duration: const Duration(milliseconds: 900),
    );
    _anim = Tween<double>(begin: 0, end: widget.value / 10)
        .animate(CurvedAnimation(parent: _ctrl, curve: Curves.easeOutCubic));

    Future.delayed(widget.delay, () {
      if (mounted) _ctrl.forward();
    });
  }

  @override
  void dispose() {
    _ctrl.dispose();
    super.dispose();
  }

  String get _levelLabel {
    if (widget.value >= 7) return 'ALTO';
    if (widget.value >= 4) return 'MEDIO';
    return 'BAJO';
  }

  @override
  Widget build(BuildContext context) {
    return Padding(
      padding: const EdgeInsets.only(bottom: 16),
      child: Row(
        children: [
          SizedBox(
            width: 110,
            child: Text(
              widget.label,
              style: const TextStyle(
                fontFamily: HVType.mono,
                fontSize: 10,
                letterSpacing: 1.2,
                color: HVColors.whiteDim,
              ),
            ),
          ),
          const SizedBox(width: 12),
          Expanded(
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                Container(
                  height: 4,
                  width: double.infinity,
                  color: HVColors.blackBorder,
                  child: AnimatedBuilder(
                    animation: _anim,
                    builder: (_, __) => FractionallySizedBox(
                      widthFactor: _anim.value,
                      alignment: Alignment.centerLeft,
                      child: Container(color: widget.color),
                    ),
                  ),
                ),
              ],
            ),
          ),
          const SizedBox(width: 12),
          SizedBox(
            width: 60,
            child: Row(
              mainAxisAlignment: MainAxisAlignment.end,
              children: [
                AnimatedBuilder(
                  animation: _anim,
                  builder: (_, __) => Text(
                    (_anim.value * 10).toStringAsFixed(1),
                    style: TextStyle(
                      fontFamily: HVType.display,
                      fontSize: 16,
                      fontWeight: FontWeight.w700,
                      color: widget.color,
                    ),
                  ),
                ),
              ],
            ),
          ),
          const SizedBox(width: 8),
          SizedBox(
            width: 40,
            child: Text(
              _levelLabel,
              style: TextStyle(
                fontFamily: HVType.mono,
                fontSize: 9,
                letterSpacing: 1.5,
                color: widget.color.withOpacity(0.7),
              ),
              textAlign: TextAlign.right,
            ),
          ),
        ],
      ),
    );
  }
}

// ──────────────────────────────────────────────────────────
// FADE + SLIDE REVEAL WIDGET
// ──────────────────────────────────────────────────────────
class HVReveal extends StatefulWidget {
  final Widget child;
  final Duration delay;
  final Offset begin;

  const HVReveal({
    super.key,
    required this.child,
    this.delay = Duration.zero,
    this.begin = const Offset(0, 0.04),
  });

  @override
  State<HVReveal> createState() => _HVRevealState();
}

class _HVRevealState extends State<HVReveal>
    with SingleTickerProviderStateMixin {
  late AnimationController _ctrl;
  late Animation<double> _fade;
  late Animation<Offset> _slide;

  @override
  void initState() {
    super.initState();
    _ctrl = AnimationController(
      vsync: this,
      duration: const Duration(milliseconds: 500),
    );
    _fade = CurvedAnimation(parent: _ctrl, curve: Curves.easeOut);
    _slide = Tween<Offset>(begin: widget.begin, end: Offset.zero)
        .animate(CurvedAnimation(parent: _ctrl, curve: Curves.easeOutCubic));

    Future.delayed(widget.delay, () {
      if (mounted) _ctrl.forward();
    });
  }

  @override
  void dispose() {
    _ctrl.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return FadeTransition(
      opacity: _fade,
      child: SlideTransition(position: _slide, child: widget.child),
    );
  }
}

// ──────────────────────────────────────────────────────────
// EVALUATION SERVICE (BUSINESS LOGIC)
// ──────────────────────────────────────────────────────────
enum EvaluationState {
  accepted,
  priority,
  conditional,
  notPriority,
  notViable,
}

class EvaluationResult {
  final double ontologico;
  final double axiologico;
  final double visibilizacion;
  final double operativo;
  final double score;
  final EvaluationState state;
  final String stateLabel;
  final String recommendation;
  final Color stateColor;
  final Color stateDimColor;

  const EvaluationResult({
    required this.ontologico,
    required this.axiologico,
    required this.visibilizacion,
    required this.operativo,
    required this.score,
    required this.state,
    required this.stateLabel,
    required this.recommendation,
    required this.stateColor,
    required this.stateDimColor,
  });
}

class EvaluationService {
  static double _norm(double raw, double min, double max) {
    if (max == min) return 5.0;
    return ((raw - min) / (max - min)) * 10.0;
  }

  static String _levelOf(double v) {
    if (v >= 7) return 'alto';
    if (v >= 4) return 'medio';
    return 'bajo';
  }

  static EvaluationResult evaluate(Map<String, int> answers) {
    // ONTOLÓGICO: precariedad del habitar
    final ontoRaw = (answers['dormir'] ?? 0) +
        (answers['refugio'] ?? 0) +
        (answers['clima'] ?? 0) +
        (answers['actividades'] ?? 0) +
        (answers['experiencia'] ?? 0) +
        (answers['espacios'] ?? 0);
    final onto = _norm(ontoRaw.toDouble(), 6, 22);

    // AXIOLÓGICO: organización colectiva
    final axioRaw = (answers['org'] ?? 0) +
        (answers['roles'] ?? 0) +
        (answers['decisiones'] ?? 0) +
        (answers['cuidado'] ?? 0) +
        (answers['recursos'] ?? 0) +
        (answers['estructuras'] ?? 0) +
        (answers['normas'] ?? 0);
    final axio = _norm(axioRaw.toDouble(), 7, 23);

    // VISIBILIZACIÓN
    final visiRaw = (answers['intencion'] ?? 0) +
        (answers['importancia'] ?? 0) +
        (answers['elementos'] ?? 0) +
        (answers['visible'] ?? 0) +
        (answers['responsables'] ?? 0) +
        (answers['claridad'] ?? 0) +
        (answers['integracion'] ?? 0);
    final visi = _norm(visiRaw.toDouble(), 7, 21);

    // OPERATIVO
    final operRaw = (answers['elementos2'] ?? 0) +
        (answers['materiales'] ?? 0) +
        (answers['modulares'] ?? 0) +
        (answers['espacio'] ?? 0);
    final oper = _norm(operRaw.toDouble(), 4, 13);

    // SCORE FINAL
    final score =
        (onto * 0.4) + (axio * 0.3) + (oper * 0.2) + (visi * 0.1);

    // ESTADO
    final ontoLv = _levelOf(onto);
    final axioLv = _levelOf(axio);
    final operLv = _levelOf(oper);
    final visiLv = _levelOf(visi);

    EvaluationState state;
    String label;
    String recommendation;
    Color color;
    Color dimColor;

    if (operLv == 'bajo') {
      state = EvaluationState.notViable;
      label = 'No viable';
      recommendation =
          'La viabilidad operativa es insuficiente para implementar estructuras. Se requiere diagnóstico de condiciones físicas antes de cualquier intervención.';
      color = HVColors.notViable;
      dimColor = HVColors.notViableDim;
    } else if (ontoLv == 'bajo' || ontoLv == 'medio') {
      state = EvaluationState.notPriority;
      label = 'No prioritario';
      recommendation =
          'El nivel de precariedad no justifica intervención inmediata. Se recomienda monitoreo y re-evaluación en 30 días.';
      color = HVColors.conditional;
      dimColor = HVColors.conditDim;
    } else if (ontoLv == 'alto' && axioLv == 'bajo') {
      state = EvaluationState.conditional;
      label = 'Condicional';
      recommendation =
          'Alta precariedad pero organización insuficiente. Se requiere acompañamiento estructurado paralelo a la implementación.';
      color = HVColors.conditional;
      dimColor = HVColors.conditDim;
    } else if (ontoLv == 'alto' && visiLv == 'alto') {
      state = EvaluationState.priority;
      label = 'Prioridad estratégica';
      recommendation =
          'Alta precariedad + alta capacidad comunicativa. Intervención con impacto territorial máximo. Activación inmediata recomendada.';
      color = HVColors.priority;
      dimColor = HVColors.priorityDim;
    } else {
      state = EvaluationState.accepted;
      label = 'Aceptado';
      recommendation =
          'Condiciones de habitabilidad, organización y viabilidad satisfactorias. Implementación estándar habilitada.';
      color = HVColors.accepted;
      dimColor = HVColors.acceptedDim;
    }

    return EvaluationResult(
      ontologico: onto,
      axiologico: axio,
      visibilizacion: visi,
      operativo: oper,
      score: score,
      state: state,
      stateLabel: label,
      recommendation: recommendation,
      stateColor: color,
      stateDimColor: dimColor,
    );
  }
}

// ──────────────────────────────────────────────────────────
// SCREEN 1: INTRO
// ──────────────────────────────────────────────────────────
class IntroScreen extends StatefulWidget {
  const IntroScreen({super.key});

  @override
  State<IntroScreen> createState() => _IntroScreenState();
}

class _IntroScreenState extends State<IntroScreen>
    with SingleTickerProviderStateMixin {
  late AnimationController _bgCtrl;

  @override
  void initState() {
    super.initState();
    _bgCtrl = AnimationController(
      vsync: this,
      duration: const Duration(milliseconds: 3000),
    )..repeat(reverse: true);
  }

  @override
  void dispose() {
    _bgCtrl.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: HVColors.black,
      body: Stack(
        children: [
          // Structural grid background
          Positioned.fill(child: _GridBackground()),

          // Red accent corner
          Positioned(
            top: 0,
            left: 0,
            child: Container(
              width: 4,
              height: MediaQuery.of(context).size.height * 0.4,
              color: HVColors.red,
            ),
          ),

          // Content
          SafeArea(
            child: Padding(
              padding: const EdgeInsets.all(28),
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  // Header mark
                  HVReveal(
                    delay: const Duration(milliseconds: 100),
                    child: Row(
                      children: [
                        Container(
                          width: 8,
                          height: 8,
                          color: HVColors.red,
                        ),
                        const SizedBox(width: 10),
                        const HVLabel('HAVITA — Sistema activo'),
                      ],
                    ),
                  ),

                  const Spacer(),

                  // Hero type
                  HVReveal(
                    delay: const Duration(milliseconds: 200),
                    child: Column(
                      crossAxisAlignment: CrossAxisAlignment.start,
                      children: [
                        Text(
                          'HABITA',
                          style: Theme.of(context)
                              .textTheme
                              .displayLarge
                              ?.copyWith(
                                fontSize: 72,
                                height: 0.9,
                                color: HVColors.white,
                              ),
                        ),
                        Text(
                          'BILIDAD',
                          style: Theme.of(context)
                              .textTheme
                              .displayLarge
                              ?.copyWith(
                                fontSize: 72,
                                height: 0.9,
                                color: HVColors.red,
                              ),
                        ),
                        Text(
                          'TEMPORAL',
                          style: Theme.of(context)
                              .textTheme
                              .displayLarge
                              ?.copyWith(
                                fontSize: 72,
                                height: 0.9,
                                color: HVColors.white.withOpacity(0.25),
                              ),
                        ),
                      ],
                    ),
                  ),

                  const SizedBox(height: 28),

                  // Descriptor
                  HVReveal(
                    delay: const Duration(milliseconds: 350),
                    child: HVCard(
                      padding: const EdgeInsets.all(16),
                      borderColor: HVColors.blackMid,
                      child: Text(
                        'Sistema de diagnóstico, clasificación y activación para colectivos en contextos de ocupación y habitabilidad temporal en espacio público.',
                        style: Theme.of(context).textTheme.bodyLarge,
                      ),
                    ),
                  ),

                  const SizedBox(height: 16),

                  // Principle quote
                  HVReveal(
                    delay: const Duration(milliseconds: 450),
                    child: Container(
                      padding: const EdgeInsets.all(16),
                      decoration: const BoxDecoration(
                        border: Border(
                          left: BorderSide(color: HVColors.red, width: 2),
                        ),
                      ),
                      child: Text(
                        '"El sistema no decide quién entra o sale, sino qué tipo de relación se establece con cada contexto."',
                        style:
                            Theme.of(context).textTheme.bodyMedium?.copyWith(
                                  fontStyle: FontStyle.italic,
                                  color: HVColors.whiteDim,
                                ),
                      ),
                    ),
                  ),

                  const SizedBox(height: 28),

                  // CTA
                  HVReveal(
                    delay: const Duration(milliseconds: 550),
                    child: HVButton(
                      label: 'Iniciar diagnóstico',
                      icon: Icons.arrow_forward,
                      onPressed: () {
                        Navigator.of(context).push(
                          _buildPageRoute(const SurveyScreen()),
                        );
                      },
                    ),
                  ),

                  const SizedBox(height: 12),

                  // Metadata
                  HVReveal(
                    delay: const Duration(milliseconds: 600),
                    child: Row(
                      children: [
                        const HVLabel('Sin login requerido'),
                        const SizedBox(width: 16),
                        Container(
                            width: 4, height: 4, color: HVColors.whiteFaint),
                        const SizedBox(width: 16),
                        const HVLabel('5 bloques · ~10 min'),
                      ],
                    ),
                  ),
                ],
              ),
            ),
          ),
        ],
      ),
    );
  }
}

// ──────────────────────────────────────────────────────────
// SCREEN 2: SURVEY
// ──────────────────────────────────────────────────────────

class SurveyQuestion {
  final String id;
  final String text;
  final String blockLabel;
  final List<SurveyOption> options;
  final bool multiSelect;

  const SurveyQuestion({
    required this.id,
    required this.text,
    required this.blockLabel,
    required this.options,
    this.multiSelect = false,
  });
}

class SurveyOption {
  final String label;
  final int value;
  const SurveyOption(this.label, this.value);
}

final List<SurveyQuestion> kSurveyQuestions = [
  // Block 1
  SurveyQuestion(
    id: 'personas',
    text: '¿Cuántas personas están ocupando el espacio?',
    blockLabel: 'B1 · DATOS GENERALES',
    options: [
      const SurveyOption('1–10 personas', 1),
      const SurveyOption('10–30 personas', 2),
      const SurveyOption('30–50 personas', 3),
      const SurveyOption('50+ personas', 4),
    ],
  ),
  SurveyQuestion(
    id: 'tiempo',
    text: '¿Hace cuánto tiempo están en el lugar?',
    blockLabel: 'B1 · DATOS GENERALES',
    options: [
      const SurveyOption('1–3 días', 1),
      const SurveyOption('1 semana', 2),
      const SurveyOption('2–4 semanas', 3),
      const SurveyOption('Más de 1 mes', 4),
    ],
  ),
  SurveyQuestion(
    id: 'modalidad',
    text: '¿La ocupación es continua o intermitente?',
    blockLabel: 'B1 · DATOS GENERALES',
    options: [
      const SurveyOption('Continua', 2),
      const SurveyOption('Intermitente', 1),
    ],
  ),
  // Block 2
  SurveyQuestion(
    id: 'dormir',
    text: '¿Cómo duermen actualmente?',
    blockLabel: 'B2 · HABITABILIDAD',
    options: [
      const SurveyOption('Suelo', 1),
      const SurveyOption('Colchonetas', 2),
      const SurveyOption('Estructuras improvisadas', 3),
      const SurveyOption('Turnos', 4),
    ],
  ),
  SurveyQuestion(
    id: 'refugio',
    text: '¿Tienen algún tipo de refugio?',
    blockLabel: 'B2 · HABITABILIDAD',
    options: [
      const SurveyOption('Ninguno', 1),
      const SurveyOption('Improvisado', 2),
      const SurveyOption('Básico', 3),
      const SurveyOption('Otro', 4),
    ],
  ),
  SurveyQuestion(
    id: 'clima',
    text: '¿Cómo se protegen del clima?',
    blockLabel: 'B2 · HABITABILIDAD',
    options: [
      const SurveyOption('No hay protección', 1),
      const SurveyOption('Parcial', 2),
      const SurveyOption('Suficiente', 3),
    ],
  ),
  SurveyQuestion(
    id: 'experiencia',
    text: '¿Cómo describirían su experiencia en el espacio?',
    blockLabel: 'B2 · HABITABILIDAD',
    options: [
      const SurveyOption('Muy precaria', 4),
      const SurveyOption('Precaria', 3),
      const SurveyOption('Media', 2),
      const SurveyOption('Estable', 1),
    ],
  ),
  SurveyQuestion(
    id: 'espacios',
    text: '¿Existen espacios diferenciados (dormir, reunirse)?',
    blockLabel: 'B2 · HABITABILIDAD',
    options: [
      const SurveyOption('No', 1),
      const SurveyOption('Parcialmente', 2),
      const SurveyOption('Sí', 3),
    ],
  ),
  // Block 3
  SurveyQuestion(
    id: 'org',
    text: '¿Existe organización interna?',
    blockLabel: 'B3 · ORGANIZACIÓN',
    options: [
      const SurveyOption('No', 1),
      const SurveyOption('Básica', 2),
      const SurveyOption('Clara', 3),
      const SurveyOption('Estructurada', 4),
    ],
  ),
  SurveyQuestion(
    id: 'decisiones',
    text: '¿Cómo toman decisiones?',
    blockLabel: 'B3 · ORGANIZACIÓN',
    options: [
      const SurveyOption('Individual', 1),
      const SurveyOption('En grupos', 2),
      const SurveyOption('Colectiva', 3),
    ],
  ),
  SurveyQuestion(
    id: 'cuidado',
    text: '¿Existe cuidado del espacio?',
    blockLabel: 'B3 · ORGANIZACIÓN',
    options: [
      const SurveyOption('No', 1),
      const SurveyOption('Mínimo', 2),
      const SurveyOption('Medio', 3),
      const SurveyOption('Alto', 4),
    ],
  ),
  SurveyQuestion(
    id: 'estructuras',
    text: '¿Están dispuestos a usar estructuras colectivas?',
    blockLabel: 'B3 · ORGANIZACIÓN',
    options: [
      const SurveyOption('No', 1),
      const SurveyOption('Tal vez', 2),
      const SurveyOption('Sí', 3),
    ],
  ),
  // Block 4
  SurveyQuestion(
    id: 'importancia',
    text: '¿Qué tan importante es ser visibles?',
    blockLabel: 'B4 · VISIBILIZACIÓN',
    options: [
      const SurveyOption('Baja', 1),
      const SurveyOption('Media', 2),
      const SurveyOption('Alta', 3),
    ],
  ),
  SurveyQuestion(
    id: 'elementos',
    text: '¿Tienen elementos de visibilización?',
    blockLabel: 'B4 · VISIBILIZACIÓN',
    options: [
      const SurveyOption('Ninguno', 1),
      const SurveyOption('Pocos', 2),
      const SurveyOption('Suficientes', 3),
    ],
  ),
  SurveyQuestion(
    id: 'visible',
    text: '¿Su mensaje es visible desde afuera?',
    blockLabel: 'B4 · VISIBILIZACIÓN',
    options: [
      const SurveyOption('No', 1),
      const SurveyOption('Parcialmente', 2),
      const SurveyOption('Sí', 3),
    ],
  ),
  SurveyQuestion(
    id: 'integracion',
    text: '¿Integrarían visibilización en los habitáculos?',
    blockLabel: 'B4 · VISIBILIZACIÓN',
    options: [
      const SurveyOption('No', 1),
      const SurveyOption('Tal vez', 2),
      const SurveyOption('Sí', 3),
    ],
  ),
  // Block 5
  SurveyQuestion(
    id: 'elementos2',
    text: '¿Qué elementos tienen actualmente?',
    blockLabel: 'B5 · OPERATIVO',
    options: [
      const SurveyOption('Ninguno', 1),
      const SurveyOption('Básicos', 2),
      const SurveyOption('Algunos', 3),
      const SurveyOption('Suficientes', 4),
    ],
  ),
  SurveyQuestion(
    id: 'materiales',
    text: '¿Cuentan con materiales propios?',
    blockLabel: 'B5 · OPERATIVO',
    options: [
      const SurveyOption('No', 1),
      const SurveyOption('Pocos', 2),
      const SurveyOption('Varios', 3),
    ],
  ),
  SurveyQuestion(
    id: 'modulares',
    text: '¿Están dispuestos a usar estructuras modulares?',
    blockLabel: 'B5 · OPERATIVO',
    options: [
      const SurveyOption('No', 1),
      const SurveyOption('Tal vez', 2),
      const SurveyOption('Sí', 3),
    ],
  ),
  SurveyQuestion(
    id: 'espacio',
    text: '¿Cuánto espacio tienen disponible?',
    blockLabel: 'B5 · OPERATIVO',
    options: [
      const SurveyOption('Reducido', 1),
      const SurveyOption('Medio', 2),
      const SurveyOption('Amplio', 3),
    ],
  ),
];

// Fill missing fields for calculation
Map<String, int> _fillDefaults(Map<String, int> answers) {
  const defaults = {
    'actividades': 2,
    'roles': 2,
    'recursos': 2,
    'normas': 2,
    'intencion': 2,
    'responsables': 2,
    'claridad': 2,
    'urgencia': 2,
  };
  final result = Map<String, int>.from(answers);
  defaults.forEach((k, v) {
    result.putIfAbsent(k, () => v);
  });
  return result;
}

class SurveyScreen extends StatefulWidget {
  const SurveyScreen({super.key});

  @override
  State<SurveyScreen> createState() => _SurveyScreenState();
}

class _SurveyScreenState extends State<SurveyScreen>
    with SingleTickerProviderStateMixin {
  int _currentIndex = 0;
  final Map<String, int> _answers = {};

  late AnimationController _progressCtrl;
  late Animation<double> _progressAnim;
  final PageController _pageController = PageController();

  @override
  void initState() {
    super.initState();
    _progressCtrl = AnimationController(
      vsync: this,
      duration: const Duration(milliseconds: 600),
    );
    _progressAnim = Tween<double>(
      begin: 0,
      end: 1 / kSurveyQuestions.length,
    ).animate(CurvedAnimation(parent: _progressCtrl, curve: Curves.easeOut));
    _progressCtrl.forward();
  }

  @override
  void dispose() {
    _progressCtrl.dispose();
    _pageController.dispose();
    super.dispose();
  }

  void _animateProgress() {
    final targetVal = (_currentIndex + 1) / kSurveyQuestions.length;
    _progressAnim = Tween<double>(
      begin: _progressAnim.value,
      end: targetVal,
    ).animate(CurvedAnimation(parent: _progressCtrl, curve: Curves.easeOut));
    _progressCtrl.reset();
    _progressCtrl.forward();
  }

  void _selectAnswer(String questionId, int value) {
    HapticFeedback.selectionClick();
    setState(() {
      _answers[questionId] = value;
    });
  }

  void _next() {
    final q = kSurveyQuestions[_currentIndex];
    if (_answers.containsKey(q.id)) {
      if (_currentIndex < kSurveyQuestions.length - 1) {
        setState(() => _currentIndex++);
        _animateProgress();
        _pageController.nextPage(
          duration: const Duration(milliseconds: 380),
          curve: Curves.easeOutCubic,
        );
      } else {
        _submit();
      }
    }
  }

  void _prev() {
    if (_currentIndex > 0) {
      setState(() => _currentIndex--);
      _animateProgress();
      _pageController.previousPage(
        duration: const Duration(milliseconds: 300),
        curve: Curves.easeOutCubic,
      );
    } else {
      Navigator.of(context).pop();
    }
  }

  void _submit() {
    final filledAnswers = _fillDefaults(_answers);
    final result = EvaluationService.evaluate(filledAnswers);
    Navigator.of(context).pushReplacement(
      _buildPageRoute(ResultScreen(result: result)),
    );
  }

  @override
  Widget build(BuildContext context) {
    final q = kSurveyQuestions[_currentIndex];
    final hasAnswer = _answers.containsKey(q.id);
    final isLast = _currentIndex == kSurveyQuestions.length - 1;

    return Scaffold(
      backgroundColor: HVColors.black,
      body: Stack(
        children: [
          Positioned.fill(child: _GridBackground(opacity: 0.4)),

          SafeArea(
            child: Column(
              children: [
                // Top navigation
                Padding(
                  padding:
                      const EdgeInsets.symmetric(horizontal: 20, vertical: 16),
                  child: Row(
                    children: [
                      GestureDetector(
                        onTap: _prev,
                        child: Container(
                          padding: const EdgeInsets.all(10),
                          decoration: BoxDecoration(
                            border:
                                Border.all(color: HVColors.blackBorder),
                          ),
                          child: const Icon(Icons.arrow_back,
                              color: HVColors.white, size: 18),
                        ),
                      ),
                      const SizedBox(width: 16),
                      Expanded(
                        child: HVProgressHeader(
                          current: _currentIndex + 1,
                          total: kSurveyQuestions.length,
                          blockLabel: q.blockLabel,
                          progressAnim: _progressAnim,
                        ),
                      ),
                    ],
                  ),
                ),

                // Page view
                Expanded(
                  child: PageView.builder(
                    controller: _pageController,
                    physics: const NeverScrollableScrollPhysics(),
                    itemCount: kSurveyQuestions.length,
                    itemBuilder: (context, index) {
                      final question = kSurveyQuestions[index];
                      return _QuestionPage(
                        question: question,
                        selectedValue: _answers[question.id],
                        onSelect: (v) => _selectAnswer(question.id, v),
                      );
                    },
                  ),
                ),

                // Bottom CTA
                Padding(
                  padding: const EdgeInsets.fromLTRB(20, 8, 20, 20),
                  child: HVButton(
                    label: isLast ? 'Ver resultado' : 'Continuar',
                    icon: isLast ? Icons.check : Icons.arrow_forward,
                    onPressed: hasAnswer ? _next : null,
                    accentColor:
                        hasAnswer ? HVColors.red : HVColors.blackMid,
                  ),
                ),
              ],
            ),
          ),
        ],
      ),
    );
  }
}

class _QuestionPage extends StatelessWidget {
  final SurveyQuestion question;
  final int? selectedValue;
  final ValueChanged<int> onSelect;

  const _QuestionPage({
    required this.question,
    required this.selectedValue,
    required this.onSelect,
  });

  @override
  Widget build(BuildContext context) {
    return SingleChildScrollView(
      padding: const EdgeInsets.symmetric(horizontal: 20),
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          // Question text
          HVReveal(
            key: ValueKey('q_${question.id}'),
            delay: const Duration(milliseconds: 40),
            child: Padding(
              padding: const EdgeInsets.only(bottom: 28, top: 8),
              child: Text(
                question.text,
                style: Theme.of(context).textTheme.headlineLarge?.copyWith(
                      fontSize: 24,
                      height: 1.25,
                      letterSpacing: 0.5,
                    ),
              ),
            ),
          ),

          // Options
          ...question.options.asMap().entries.map((entry) {
            final idx = entry.key;
            final opt = entry.value;
            final isSelected = selectedValue == opt.value;

            return HVReveal(
              key: ValueKey('opt_${question.id}_$idx'),
              delay: Duration(milliseconds: 80 + idx * 60),
              child: Padding(
                padding: const EdgeInsets.only(bottom: 10),
                child: HVChip(
                  label: opt.label,
                  selected: isSelected,
                  onTap: () => onSelect(opt.value),
                ),
              ),
            );
          }),

          const SizedBox(height: 16),
        ],
      ),
    );
  }
}

// ──────────────────────────────────────────────────────────
// SCREEN 3: RESULT
// ──────────────────────────────────────────────────────────
class ResultScreen extends StatefulWidget {
  final EvaluationResult result;

  const ResultScreen({super.key, required this.result});

  @override
  State<ResultScreen> createState() => _ResultScreenState();
}

class _ResultScreenState extends State<ResultScreen>
    with SingleTickerProviderStateMixin {
  late AnimationController _heroCtrl;
  late Animation<double> _heroScale;
  late Animation<double> _heroFade;

  @override
  void initState() {
    super.initState();
    _heroCtrl = AnimationController(
      vsync: this,
      duration: const Duration(milliseconds: 700),
    );
    _heroScale = Tween<double>(begin: 0.92, end: 1.0).animate(
      CurvedAnimation(parent: _heroCtrl, curve: Curves.easeOutCubic),
    );
    _heroFade = CurvedAnimation(parent: _heroCtrl, curve: Curves.easeOut);

    Future.delayed(const Duration(milliseconds: 100), () {
      if (mounted) _heroCtrl.forward();
    });
  }

  @override
  void dispose() {
    _heroCtrl.dispose();
    super.dispose();
  }

  String get _accessDescription {
    switch (widget.result.state) {
      case EvaluationState.accepted:
      case EvaluationState.priority:
        return 'Acceso completo habilitado. Puede registrarse y acceder a herramientas y recursos de implementación.';
      case EvaluationState.conditional:
        return 'Acceso con acompañamiento. Registro habilitado con flujo guiado de implementación condicionada.';
      case EvaluationState.notPriority:
        return 'Acceso limitado. Se muestran recomendaciones y se habilita re-evaluación en 30 días.';
      case EvaluationState.notViable:
        return 'Acceso informativo. Se explica el diagnóstico y no se habilita implementación en esta instancia.';
    }
  }

  String get _priorityLabel {
    if (widget.result.score >= 7) return 'ALTA';
    if (widget.result.score >= 4) return 'MEDIA';
    return 'BAJA';
  }

  @override
  Widget build(BuildContext context) {
    final r = widget.result;

    return Scaffold(
      backgroundColor: HVColors.black,
      body: Stack(
        children: [
          Positioned.fill(child: _GridBackground(opacity: 0.3)),

          // Accent glow top
          Positioned(
            top: -80,
            left: -40,
            child: Container(
              width: 300,
              height: 300,
              decoration: BoxDecoration(
                shape: BoxShape.circle,
                color: r.stateColor.withOpacity(0.06),
              ),
            ),
          ),

          // Content
          SafeArea(
            child: CustomScrollView(
              slivers: [
                // App bar
                SliverToBoxAdapter(
                  child: Padding(
                    padding: const EdgeInsets.fromLTRB(20, 16, 20, 0),
                    child: Row(
                      children: [
                        GestureDetector(
                          onTap: () => Navigator.of(context)
                              .popUntil((r) => r.isFirst),
                          child: Container(
                            padding: const EdgeInsets.all(10),
                            decoration: BoxDecoration(
                              border: Border.all(color: HVColors.blackBorder),
                            ),
                            child: const Icon(Icons.close,
                                color: HVColors.white, size: 16),
                          ),
                        ),
                        const SizedBox(width: 14),
                        const HVLabel('DIAGNÓSTICO COMPLETADO'),
                      ],
                    ),
                  ),
                ),

                // Hero result block
                SliverToBoxAdapter(
                  child: Padding(
                    padding: const EdgeInsets.fromLTRB(20, 24, 20, 20),
                    child: ScaleTransition(
                      scale: _heroScale,
                      child: FadeTransition(
                        opacity: _heroFade,
                        child: Container(
                          padding: const EdgeInsets.all(24),
                          decoration: BoxDecoration(
                            color: r.stateDimColor,
                            border:
                                Border.all(color: r.stateColor, width: 1.0),
                          ),
                          child: Column(
                            crossAxisAlignment: CrossAxisAlignment.start,
                            children: [
                              Row(
                                mainAxisAlignment:
                                    MainAxisAlignment.spaceBetween,
                                crossAxisAlignment: CrossAxisAlignment.start,
                                children: [
                                  Column(
                                    crossAxisAlignment:
                                        CrossAxisAlignment.start,
                                    children: [
                                      Text(
                                        'ESTADO',
                                        style: TextStyle(
                                          fontFamily: HVType.mono,
                                          fontSize: 10,
                                          letterSpacing: 2.5,
                                          color:
                                              r.stateColor.withOpacity(0.7),
                                        ),
                                      ),
                                      const SizedBox(height: 6),
                                      Text(
                                        r.stateLabel.toUpperCase(),
                                        style: TextStyle(
                                          fontFamily: HVType.display,
                                          fontSize: 34,
                                          fontWeight: FontWeight.w900,
                                          letterSpacing: 1.0,
                                          height: 1.0,
                                          color: r.stateColor,
                                        ),
                                      ),
                                    ],
                                  ),
                                  Column(
                                    crossAxisAlignment:
                                        CrossAxisAlignment.end,
                                    children: [
                                      Text(
                                        'SCORE',
                                        style: TextStyle(
                                          fontFamily: HVType.mono,
                                          fontSize: 10,
                                          letterSpacing: 2.5,
                                          color:
                                              r.stateColor.withOpacity(0.7),
                                        ),
                                      ),
                                      const SizedBox(height: 4),
                                      Text(
                                        r.score.toStringAsFixed(1),
                                        style: TextStyle(
                                          fontFamily: HVType.display,
                                          fontSize: 48,
                                          fontWeight: FontWeight.w900,
                                          height: 1.0,
                                          color: r.stateColor,
                                        ),
                                      ),
                                      Text(
                                        'PRIORIDAD $_priorityLabel',
                                        style: TextStyle(
                                          fontFamily: HVType.mono,
                                          fontSize: 10,
                                          letterSpacing: 1.5,
                                          color:
                                              r.stateColor.withOpacity(0.7),
                                        ),
                                      ),
                                    ],
                                  ),
                                ],
                              ),
                            ],
                          ),
                        ),
                      ),
                    ),
                  ),
                ),

                // Indices
                SliverToBoxAdapter(
                  child: HVReveal(
                    delay: const Duration(milliseconds: 300),
                    child: Padding(
                      padding:
                          const EdgeInsets.symmetric(horizontal: 20),
                      child: HVCard(
                        child: Column(
                          crossAxisAlignment: CrossAxisAlignment.start,
                          children: [
                            const HVLabel('ÍNDICES DE EVALUACIÓN'),
                            const SizedBox(height: 20),
                            HVIndexBar(
                              label: 'ONTOLÓGICO',
                              value: r.ontologico,
                              color: _indexColor(r.ontologico),
                              delay: const Duration(milliseconds: 400),
                            ),
                            HVIndexBar(
                              label: 'AXIOLÓGICO',
                              value: r.axiologico,
                              color: _indexColor(r.axiologico),
                              delay: const Duration(milliseconds: 500),
                            ),
                            HVIndexBar(
                              label: 'VISIBILIZACIÓN',
                              value: r.visibilizacion,
                              color: _indexColor(r.visibilizacion),
                              delay: const Duration(milliseconds: 600),
                            ),
                            HVIndexBar(
                              label: 'OPERATIVO',
                              value: r.operativo,
                              color: _indexColor(r.operativo),
                              delay: const Duration(milliseconds: 700),
                            ),
                          ],
                        ),
                      ),
                    ),
                  ),
                ),

                const SliverToBoxAdapter(child: SizedBox(height: 12)),

                // Recommendation
                SliverToBoxAdapter(
                  child: HVReveal(
                    delay: const Duration(milliseconds: 500),
                    child: Padding(
                      padding:
                          const EdgeInsets.symmetric(horizontal: 20),
                      child: HVCard(
                        borderColor: r.stateColor.withOpacity(0.3),
                        child: Column(
                          crossAxisAlignment: CrossAxisAlignment.start,
                          children: [
                            const HVLabel('RECOMENDACIÓN'),
                            const SizedBox(height: 12),
                            Text(
                              r.recommendation,
                              style: Theme.of(context)
                                  .textTheme
                                  .titleLarge
                                  ?.copyWith(
                                    color: HVColors.white,
                                    height: 1.5,
                                    fontSize: 15,
                                  ),
                            ),
                          ],
                        ),
                      ),
                    ),
                  ),
                ),

                const SliverToBoxAdapter(child: SizedBox(height: 12)),

                // Access level
                SliverToBoxAdapter(
                  child: HVReveal(
                    delay: const Duration(milliseconds: 650),
                    child: Padding(
                      padding:
                          const EdgeInsets.symmetric(horizontal: 20),
                      child: Column(
                        crossAxisAlignment: CrossAxisAlignment.start,
                        children: [
                          Padding(
                            padding: const EdgeInsets.only(bottom: 10),
                            child: HVLabel(
                              'NIVEL DE ACCESO HABILITADO',
                              color: r.stateColor.withOpacity(0.8),
                            ),
                          ),
                          Container(
                            padding: const EdgeInsets.all(16),
                            decoration: BoxDecoration(
                              border: Border.all(
                                color: r.stateColor.withOpacity(0.25),
                              ),
                            ),
                            child: Row(
                              children: [
                                Container(
                                  width: 8,
                                  height: 8,
                                  color: r.stateColor,
                                ),
                                const SizedBox(width: 14),
                                Expanded(
                                  child: Text(
                                    _accessDescription,
                                    style: Theme.of(context)
                                        .textTheme
                                        .bodyMedium,
                                  ),
                                ),
                              ],
                            ),
                          ),
                        ],
                      ),
                    ),
                  ),
                ),

                // Philosophy
                SliverToBoxAdapter(
                  child: HVReveal(
                    delay: const Duration(milliseconds: 750),
                    child: Padding(
                      padding: const EdgeInsets.fromLTRB(20, 20, 20, 12),
                      child: Container(
                        padding: const EdgeInsets.all(16),
                        decoration: const BoxDecoration(
                          border: Border(
                            left: BorderSide(color: HVColors.red, width: 2),
                          ),
                        ),
                        child: Text(
                          '"La decisión no se basa únicamente en la necesidad, sino en la relación entre precariedad, capacidad colectiva y viabilidad de implementación."',
                          style:
                              Theme.of(context).textTheme.bodyMedium?.copyWith(
                                    fontStyle: FontStyle.italic,
                                    color: HVColors.whiteDim,
                                  ),
                        ),
                      ),
                    ),
                  ),
                ),

                // CTA buttons
                SliverToBoxAdapter(
                  child: HVReveal(
                    delay: const Duration(milliseconds: 850),
                    child: Padding(
                      padding: const EdgeInsets.fromLTRB(20, 8, 20, 32),
                      child: Column(
                        children: [
                          if (r.state == EvaluationState.accepted ||
                              r.state == EvaluationState.priority ||
                              r.state == EvaluationState.conditional)
                            HVButton(
                              label: 'Continuar al sistema',
                              icon: Icons.arrow_forward,
                              onPressed: () {},
                            )
                          else
                            HVButton(
                              label: 'Solicitar nueva evaluación',
                              icon: Icons.refresh,
                              outlined: true,
                              onPressed: () {
                                Navigator.of(context)
                                    .popUntil((r) => r.isFirst);
                              },
                            ),
                          const SizedBox(height: 10),
                          HVButton(
                            label: 'Volver al inicio',
                            outlined: true,
                            onPressed: () {
                              Navigator.of(context)
                                  .popUntil((r) => r.isFirst);
                            },
                          ),
                        ],
                      ),
                    ),
                  ),
                ),
              ],
            ),
          ),
        ],
      ),
    );
  }

  Color _indexColor(double v) {
    if (v >= 7) return HVColors.accepted;
    if (v >= 4) return HVColors.conditional;
    return HVColors.notViable;
  }
}

// ──────────────────────────────────────────────────────────
// BACKGROUND GRID DECORATION
// ──────────────────────────────────────────────────────────
class _GridBackground extends StatelessWidget {
  final double opacity;
  const _GridBackground({this.opacity = 0.6});

  @override
  Widget build(BuildContext context) {
    return CustomPaint(
      painter: _GridPainter(opacity: opacity),
    );
  }
}

class _GridPainter extends CustomPainter {
  final double opacity;
  const _GridPainter({required this.opacity});

  @override
  void paint(Canvas canvas, Size size) {
    final paint = Paint()
      ..color = HVColors.blackBorder.withOpacity(opacity)
      ..strokeWidth = 0.5;

    const spacing = 36.0;

    for (double x = 0; x <= size.width; x += spacing) {
      canvas.drawLine(Offset(x, 0), Offset(x, size.height), paint);
    }
    for (double y = 0; y <= size.height; y += spacing) {
      canvas.drawLine(Offset(0, y), Offset(size.width, y), paint);
    }

    // Intersection dots
    final dotPaint = Paint()
      ..color = HVColors.blackMid.withOpacity(opacity)
      ..style = PaintingStyle.fill;

    for (double x = 0; x <= size.width; x += spacing) {
      for (double y = 0; y <= size.height; y += spacing) {
        canvas.drawCircle(Offset(x, y), 1.0, dotPaint);
      }
    }
  }

  @override
  bool shouldRepaint(covariant _GridPainter old) => old.opacity != opacity;
}

// ──────────────────────────────────────────────────────────
// PAGE ROUTE TRANSITION
// ──────────────────────────────────────────────────────────
PageRouteBuilder _buildPageRoute(Widget screen) {
  return PageRouteBuilder(
    transitionDuration: const Duration(milliseconds: 500),
    reverseTransitionDuration: const Duration(milliseconds: 300),
    pageBuilder: (_, __, ___) => screen,
    transitionsBuilder: (_, animation, __, child) {
      return FadeTransition(
        opacity: CurvedAnimation(parent: animation, curve: Curves.easeOut),
        child: SlideTransition(
          position: Tween<Offset>(
            begin: const Offset(0.04, 0),
            end: Offset.zero,
          ).animate(
            CurvedAnimation(parent: animation, curve: Curves.easeOutCubic),
          ),
          child: child,
        ),
      );
    },
  );
}
