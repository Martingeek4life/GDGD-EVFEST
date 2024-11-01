# GDG-DEVFEST
# Understand google translate and building machiine translation for low resources languages, @Martin Rodrigue Atangana Ongolo, e-mail: atanganamr@yahoo.com

Documentation

git clone this link: https://github.com/wengong-jin/fairseq-py or https://github.com/sliedes/fairseq-py

## Generer les BPE
1-	Learn BPE

subword-nmt learn-bpe -s 4000 < new_testament_ew_train.txt > bpe_codes_ew

subword-nmt learn-bpe -s 4000 < new_testament_fr_train.txt > bpe_codes_fr

2-	Apply BPE

subword-nmt apply-bpe -c bpe_codes_ew < new_testament_ew_train.txt > train.bpe.ew

subword-nmt apply-bpe -c bpe_codes_fr < new_testament_fr_train.txt > train.bpe.fr

subword-nmt apply-bpe -c bpe_codes_ew < new_testament_ew_test.txt > test.bpe.ew

subword-nmt apply-bpe -c bpe_codes_ew < new_testament_ew_dev.txt > dev.bpe.ew

subword-nmt apply-bpe -c bpe_codes_fr < new_testament_fr_test.txt > test.bpe.fr

subword-nmt apply-bpe -c bpe_codes_fr < new_testament_fr_dev.txt > dev.bpe.fr

## Preprocess Data for Fairseq

Fairseq requires data to be in a binary format:

preprocess.py --source-lang ew --target-lang fr --trainpref train.bpe --validpref dev.bpe --testpref test.bpe --destdir data-bin

## Train the model with conv architecture good for low resources

python3 train.py data-bin/ --arch fconv_iwslt_de_en     --optimizer adam     --lr 0.0005     --clip-norm 0.1     --dropout 0.3     --max-tokens 4000     --lr-scheduler reduce_lr_on_plateau     --lr-shrink 0.5     --criterion label_smoothed_cross_entropy     --label-smoothing 0.1     --max-epoch 20     --save-dir checkpoints/  --log-format json     --log-interval 10     --seed 42  --encoder-embed-dim 256  --encoder-layers 4     --decoder-embed-dim 256         --decoder-layers 4     --batch-size 32 --adam-betas "(0.9, 0.98)"



## Generate translation
python3 generate.py data-bin/ --path checkpoints/checkpoint_best.pt --batch-size 128 --beam 5 --remove-bpe



