function validarCartao(numeroCartao) {
    // Remove espaços ou caracteres não numéricos
    const numero = numeroCartao.replace(/\D/g, '');

    // Verifica a bandeira do cartão
    let bandeira = '';

    if (/^4/.test(numero)) {
        bandeira = 'Visa';
    } else if (/^5[1-5]/.test(numero) || /^2(2[2-9][1-9]|2[3-9]\d|[3-6]\d{2}|7[0-1]\d|720)/.test(numero)) {
        bandeira = 'MasterCard';
    } else if (/^(4011|4312|4389|5041|5066|5090|6277|6362|6363)/.test(numero)) {
        bandeira = 'Elo';
    } else if (/^3[47]/.test(numero)) {
        bandeira = 'American Express';
    } else if (/^6011|^65|^64[4-9]/.test(numero)) {
        bandeira = 'Discover';
    } else if (/^6062/.test(numero)) {
        bandeira = 'Hipercard';
    } else {
        bandeira = 'Bandeira desconhecida';
    }

    // Validação do número do cartão usando o algoritmo de Luhn
    const valido = validarLuhn(numero);

    return {
        bandeira,
        valido
    };
}

function validarLuhn(numero) {
    let soma = 0;
    let alternar = false;

    for (let i = numero.length - 1; i >= 0; i--) {
        let digito = parseInt(numero[i], 10);

        if (alternar) {
            digito *= 2;
            if (digito > 9) {
                digito -= 9;
            }
        }

        soma += digito;
        alternar = !alternar;
    }

    return soma % 10 === 0;
}

// Exemplo de uso:
const resultado = validarCartao('4111111111111111'); // Número de exemplo Visa
console.log(`Bandeira: ${resultado.bandeira}, Válido: ${resultado.valido}`);
